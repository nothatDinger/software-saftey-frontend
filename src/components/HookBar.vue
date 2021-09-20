<template>
  <div class="side_bar">
   <!-- <li v-for="hook in items" :key = hook >
      <input type="button" class = "button" @click="hook.toggled = !hook.toggled">
      <input type="checkbox" class="checkbox">
      <span>{{hook.hook_class}}</span>

      <div class="second_list" v-show="hook.toggled">
        <ul>
          <li v-for="item in hook.hook_items" :key = item>
            <input type="checkbox" class="checkbox">
            <span>{{item}}</span>
          </li>
        </ul>
      </div>
  </li>-->

    <q-tree class="col-12 col-sm-6"
            :nodes="items"
            node-key="label"
            tick-strategy="leaf"
            no-connectors
            v-model:selected= "selected"
            v-model:ticked="ticked"
            v-model:expanded="expanded"
    />

  </div>

</template>


<script>
import { ionChevronUp } from '@quasar/extras/ionicons-v5'
import { ref } from 'vue'
export default {
  name: 'HookBar',
  methods:{
  },
  created(){
    this.ionChevronUp = ionChevronUp
  },
  data() {
    return {
      selected: ref('Send'),
      ticked: ref(['HeapCreate','Registry']),
      expanded: ['Registry'],
      items: [
        {label: "MessageBox", children:[{label:"MessageBoxA"}, {label:"MessageBoxW"}]},
        {label: "Heap", children: [{label:"HeapCreate"}, {label:"HeapAlloc"}, {label:"HeapDestroy"}]},
        {label: "Socket", children: [{label:"Bind"}, {label:"Send"}, {label:"Connect"}, {label:"socket"},{label:"Recv"}]},
        {label: "File", children: [{label:"CreateFile"}, {label:"ReadFile"}, {label:"WriteFile"}]},
        {label: "Registry", children: [{label:"RegCreateKeyEx"},{label:"RegSetValueEx"},{label:"RegCloseKey"},{label:"RegOpenKeyEx"},{label:"RegDeleteValue"}]},
        {label: "Memcpy", children:[{label:"Memcpy"}]}
      ]
    }
  },
  method:{
    return_ticked(){
      this.$emit('return_val', this.items)
    }
  }
}
</script>

<style scoped>



</style>
