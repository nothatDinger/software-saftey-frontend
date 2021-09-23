<template>
  <div id="top_bar" class="row no-wrap">

    <q-file
      v-model="files"
      label="Pick files"
      outlined
      class="col-6 q-pr-lg"
    >
<!--      <template v-slot:file="{ index, file }">-->
<!--        <q-chip-->
<!--          class="full-width q-my-xs"-->
<!--          :removable="isUploading && uploadProgress[index].percent < 1"-->
<!--          square-->
<!--          @remove="cancelFile(index)"-->
<!--        >-->
<!--          <q-linear-progress-->
<!--            class="absolute-full full-height"-->
<!--            :value="uploadProgress[index].percent"-->
<!--            :color="uploadProgress[index].color"-->
<!--            track-color="grey-2"-->
<!--          />-->

<!--          <q-avatar>-->
<!--            <q-icon :name="uploadProgress[index].icon" />-->
<!--          </q-avatar>-->

<!--          <div class="ellipsis relative-position">-->
<!--            {{ file.name }}-->
<!--          </div>-->

<!--          <q-tooltip>-->
<!--            {{ file.name }}-->
<!--          </q-tooltip>-->
<!--        </q-chip>-->
<!--      </template>-->

<!--      <template v-slot:after v-if="canUpload">-->
<!--        <q-btn-->
<!--          color="primary"-->
<!--          dense-->
<!--          icon="cloud_upload"-->
<!--          round-->
<!--          @click="upload"-->
<!--          :disable="!canUpload"-->
<!--          :loading="isUploading"-->
<!--        />-->
<!--      </template>-->
    </q-file>

    <q-btn
      :loading="loading1"
      color="grey"
      @click="checkAndSend"
      label="Start"
      padding="10px 10px"
      size="10px"
      align="around"
      class="col-1"
    />

  </div>
</template>

<script>
import {reactive, toRefs, computed} from "vue";

export default {
  name: "TopBar",
  // computed: {
  //   isUploading () {
  //     return this.uploading !== null
  //   },
  //
  //   canUpload () {
  //     return this.files !== null
  //   }
  // },
  // data (){
  //   let loading1;
  //    return {
  //      loading1: false,
  //      files: null,
  //      uploadProgress: [],
  //      uploading: null
  //   }
  // },
  emits:['send'],
  setup(props, context){

    const self_data = reactive({
      loading1: false,
      files: null,
    })
    const methods ={

      checkAndSend(){
        self_data.loading1 = true
        // simulate a delay
        if(self_data.files.path == null){
          alert("请在桌面应用环境下使用本程序，否则无法获取文件路径")
        }
       // console.log(this.files)
        let i =0 ;
        let fader = setInterval(()=>{
          if(self_data.files != null) {
            self_data.loading1 = false
            clearInterval(fader)
            context.emit('send', self_data.files, true)
          }
          i++
          if (i>=5){
            self_data.loading1 = false
            clearInterval(fader)
            context.emit('send', null, false)
          }
        },500)
      }
    }
    return {...methods, ...toRefs(self_data)}
  },

}
</script>
<style scoped>

</style>
