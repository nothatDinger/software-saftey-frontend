

<template>
  <div id="main_body">
  <q-table
    :rows="rows"
    :columns="columns"
    hide-pagination
    :pagination="pagination"
    row-key="time"
  >
    <template v-slot:header="props">
      <q-tr :props="props">
        <q-th auto-width />
        <q-th
          v-for="col in props.cols"
          :key="col.name"
          align="center"
        >
          {{ col.label }}
        </q-th>
      </q-tr>
    </template>

    <template v-slot:body="props">
      <q-tr :props="props">
        <q-td auto-width>

          <q-btn size="sm"  round @click="props.expand = !props.expand" :icon="props.expand ? ionChevronDown : ionChevronForward" />
        </q-td>
        <q-td
          v-for="col in props.cols"
          :key="col.name"
          align="center"
        >
          {{ col.value }}
        </q-td>
      </q-tr>

      <q-tr v-show="props.expand" :props="props">
        <q-td colspan="100%">
          <q-list separator dense>

            <q-item v-for="item in props.row.args" :key="item">
              <q-item-section align="center">
                <q-item-label>{{item.argName+':'}}</q-item-label>
              </q-item-section>
              <q-item-section align="center">
                <q-item-label>{{item.argValue}}</q-item-label>
              </q-item-section>
            </q-item>

          </q-list>
        </q-td>
      </q-tr>
    </template>


  </q-table>

  </div>
</template>
<script>

import { ionChevronForwardOutline } from '@quasar/extras/ionicons-v5'
import {ionChevronDownOutline } from "@quasar/extras/ionicons-v5";

export default {
  name: "MainBody",
  props:['rows'],
  created(){
    this.ionChevronForward = ionChevronForwardOutline
    this.ionChevronDown = ionChevronDownOutline
  },
  data(){
    return{
      pagination:{
        rowsPerPage: 0
      },
      columns:[
        {name:"HookedAPI",label:"Hooked API",align:"left",field:row=>row.name},
        {name:"HookedTime",label: "Hooked Time",align: "center", field:r => r.time}
      ],
      // rows:[
      //   {name:"MessageBoxA", time:"16:10",args:[{argName:"test",argValue:"test"},{argName:"test2",argValue:"test2"},{argName:"test3",argValue:"test3"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
      //   {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]}
      // ],
      second_column:[
        {name:"argName",label:"argName",align:"left",field:r=>r.argName},
        {name:"argValue",label:"argValue",align:"left",field:r=>r.argValue},
      ]
    }
  },
  methods:{
    getSelectedString () {
      return '' // 不返回空时，table自带的左下角显示为默认的文字
    }
  }
}
</script>
<style scoped>


</style>
