<template>
  <div id="action_analyse">
    <q-timeline color="primary">

<!--      <q-item>-->
<!--        <q-chip :icon="ionAlertCircleOutline">-->
<!--          Action Analyse-->
<!--          <q-badge color="red" floating transparent rounded>3</q-badge>-->
<!--        </q-chip>-->
<!--      </q-item>-->

      <q-field color="grey-3"  rounded outlined stack-label>
        <template v-slot:prepend>
          <q-icon :name="ionAlertCircleOutline" color="red" />
        </template>
        <template v-slot:control>
          <div class="self-center full-width no-outline" tabindex="0" align="center" style="font-size: 17px">Action Analyse</div>
        </template>
        <q-badge color="red" floating transparent rounded>3</q-badge>
      </q-field>
      <q-timeline-entry v-for="item in warningText" :key="item"
        :subtitle="item.time" :body="item.text"
      >



      </q-timeline-entry>
    </q-timeline>
  </div>
</template>

<script>
import { ionAlertCircleOutline } from '@quasar/extras/ionicons-v5'
export default {
  name: "ActionAnalyse",
  props :['warningText'],
  created() {
    this.ionAlertCircleOutline = ionAlertCircleOutline
  },
  data(){

    return{
      // warningText:[{time:"19:00",text:"there is a message alert "},{time:"19:02",text:"there is something wrong"},{time:"19:03",text:"HeapAllocating !"}]
    }
  }
}
</script>
<style scoped>

</style>
