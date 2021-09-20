<template>
  <div id="process_info">

<!--  <div class="row">-->
<!--    <q-field outlined stack-label>-->
<!--    <template v-slot:control>-->
<!--      <span class="self-center no-outline">processName</span>-->
<!--    </template>-->
<!--  </q-field>-->
<!--    <div style="width: 40px"></div>-->
<!--    <q-field outlined  stack-label >-->
<!--      <template v-slot:control>-->
<!--        <div class="self-center no-outline"  style="min-width: 80px">{{processName}}</div>-->
<!--      </template>-->
<!--    </q-field>-->
<!--  </div>-->
<!--    <div class="row">-->
<!--      <q-field outlined stack-label>-->
<!--        <template v-slot:control>-->
<!--          <span class="self-center no-outline">processID</span>-->
<!--        </template>-->
<!--      </q-field>-->
<!--      <div style="width: 40px"></div>-->
<!--      <q-field outlined  stack-label >-->
<!--        <template v-slot:control>-->
<!--          <div class="self-center no-outline"  style="min-width: 80px">{{processID}}</div>-->
<!--        </template>-->
<!--      </q-field>-->
<!--    </div>-->
<!--    <div class="row">-->
<!--      <q-field outlined stack-label>-->
<!--        <template v-slot:control>-->
<!--          <span class="self-center no-outline">process priority</span>-->
<!--        </template>-->
<!--      </q-field>-->
<!--      <div style="width: 40px"></div>-->
<!--      <q-field outlined  stack-label >-->
<!--        <template v-slot:control>-->
<!--          <div class="self-center no-outline"  style="min-width: 80px">{{processPriority}}</div>-->
<!--        </template>-->
<!--      </q-field>-->
<!--    </div>-->
  <q-list>
    <q-item>
      <q-item-section>
        <q-field outlined stack-label>
          <template v-slot:control>
            <span class="self-center no-outline">processName:</span>
          </template>
        </q-field>
      </q-item-section>
      <q-item-section>
        <q-field outlined  stack-label >
          <template v-slot:control>
            <div class="self-center no-outline" >{{processInfo['processName']}}</div>
          </template>
        </q-field>
      </q-item-section>
    </q-item>
    <q-item>
      <q-item-section>
        <q-field outlined stack-label>
          <template v-slot:control>
            <span class="self-center no-outline">processID:</span>
          </template>
        </q-field>
      </q-item-section>
      <q-item-section>
        <q-field outlined  stack-label >
          <template v-slot:control>
            <div class="self-center no-outline" >{{processInfo['processID']}}</div>
          </template>
        </q-field>
      </q-item-section>
    </q-item>
    <q-item>
      <q-item-section>
        <q-field outlined stack-label>
          <template v-slot:control>
            <span class="self-center no-outline">process priority:</span>
          </template>
        </q-field>
      </q-item-section>
      <q-item-section>
        <q-field outlined  stack-label >
          <template v-slot:control>
            <div class="self-center no-outline"  >{{processInfo['processPriority']}}</div>
          </template>
        </q-field>
      </q-item-section>
    </q-item>
    <q-item>
      <q-item-section>
        <q-field outlined  stack-label style="max-width: 180px">
          <template v-slot:control>
            <span class="self-center no-outline" >process Modules:</span>
          </template>
        </q-field>
      </q-item-section>
    </q-item>
    <q-item>
      <q-item-section>
        <q-field outlined  stack-label >
          <template v-slot:control>
            <q-list separator>

              <q-item v-for="i in processInfo['processModule']" :key="i">
                <q-item-section>
                  <q-item-label>{{i}}</q-item-label>
                </q-item-section>
              </q-item>


            </q-list>
          </template>
        </q-field>
      </q-item-section>
    </q-item>
  </q-list>
  </div>
</template>

<script>
export default {
  name: "ProcessInfo",
  props:['processInfo'],
  data(){
  }
}
</script>
<style scoped >
.flex-break{
  flex: 1 0 100% !important;
  height: 0;
}


</style>
