<template>
  <div id="top_bar" class="row no-wrap">

    <q-file
      :value="files"
      @input="updateFiles"
      label="Pick files"
      outlined
      multiple
      :clearable="!isUploading"
      class="col-6 q-pr-lg"
    >
      <template v-slot:file="{ index, file }">
        <q-chip
          class="full-width q-my-xs"
          :removable="isUploading && uploadProgress[index].percent < 1"
          square
          @remove="cancelFile(index)"
        >
          <q-linear-progress
            class="absolute-full full-height"
            :value="uploadProgress[index].percent"
            :color="uploadProgress[index].color"
            track-color="grey-2"
          />

          <q-avatar>
            <q-icon :name="uploadProgress[index].icon" />
          </q-avatar>

          <div class="ellipsis relative-position">
            {{ file.name }}
          </div>

          <q-tooltip>
            {{ file.name }}
          </q-tooltip>
        </q-chip>
      </template>

      <template v-slot:after v-if="canUpload">
        <q-btn
          color="primary"
          dense
          icon="cloud_upload"
          round
          @click="upload"
          :disable="!canUpload"
          :loading="isUploading"
        />
      </template>
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
export default {
  name: "TopBar",
  computed: {
    isUploading () {
      return this.uploading !== null
    },

    canUpload () {
      return this.files !== null
    }
  },
  data (){
     return {
       loading1: false,
       files: null,
       uploadProgress: [],
       uploading: null
    }
  },
  methods: {
    checkAndSend(){
      this[`loading1`] = true
      // simulate a delay
      let i =0 ;
      let fader = setInterval(()=>{
          if(this.files != null) {
            this[`loading1`] = false
            clearInterval(fader)
            this.$emit('send', this.files, true)
          }
          i++
          if (i>=5){
            this[`loading1`] = false
            clearInterval(fader)
            this.$emit('send', null, false)
          }
      },500)

    },
    cancelFile (index) {
      this.uploadProgress[index] = {
        ...this.uploadProgress[index],
        error: true,
        color: 'orange-2'
      }
    },
    updateFiles (files) {
      this.files = files
      this.uploadProgress = (files || []).map(file => ({
        error: false,
        color: 'green-2',
        percent: 0,
        icon: file.type.indexOf('video/') === 0
          ? 'movie'
          : (file.type.indexOf('image/') === 0
              ? 'photo'
              : (file.type.indexOf('audio/') === 0
                  ? 'audiotrack'
                  : 'insert_drive_file'
              )
          )
      }))
    },
    upload () {
      clearTimeout(this.uploading)

      const allDone = this.uploadProgress.every(progress => progress.percent === 1)

      this.uploadProgress = this.uploadProgress.map(progress => ({
        ...progress,
        error: false,
        color: 'green-2',
        percent: allDone === true ? 0 : progress.percent
      }))

      this.__updateUploadProgress()
    },

    __updateUploadProgress () {
      let done = true

      this.uploadProgress = this.uploadProgress.map(progress => {
        if (progress.percent === 1 || progress.error === true) {
          return progress
        }

        const percent = Math.min(1, progress.percent + Math.random() / 10)
        const error = percent < 1 && Math.random() > 0.95

        if (error === false && percent < 1 && done === true) {
          done = false
        }

        return {
          ...progress,
          error,
          color: error === true ? 'red-2' : 'green-2',
          percent
        }
      })

      this.uploading = done !== true
        ? setTimeout(this.__updateUploadProgress, 300)
        : null
    },

  },
}
</script>
<style scoped>

</style>
