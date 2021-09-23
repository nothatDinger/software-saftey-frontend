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
        <q-badge color="red" floating transparent rounded>{{warning_text.length}}</q-badge>
      </q-field>
      <q-timeline-entry v-for="item in warning_text" :key="item"
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
  props :['warning_text'],
  created() {
    this.ionAlertCircleOutline = ionAlertCircleOutline
  },

}
</script>
<style scoped>

</style>
