<script>
import Recorder from 'recorder-core/recorder.wav.min.js'
import { saveAs } from 'file-saver'
export default {
  name: 'App',
  data: () =>({
    rec: null,
    duration: 0
  }),
  mounted() {
    this.initRec()
  },
  methods: {
    initRec() {
      this.rec=Recorder({
        type:"wav",sampleRate:16000,bitRate:16,
        onProcess: (buffers,powerLevel,bufferDuration,bufferSampleRate,newBufferIdx,asyncEnd) =>{
          this.duration = bufferDuration/1000
        }
      })

      this.rec.open(() =>{

      },(msg,isUserNotAllow) =>{
        console.log((isUserNotAllow?"UserNotAllow，":"")+"无法录音:"+msg);
      })
    },
    recStart() {

      this.rec.start()
    },
    recStop() {
      this.rec.stop(blob => {
        saveAs(blob, "rec.wav");

        console.log(this.duration)
        console.log(Math.floor(this.duration))

        this.duration = 0
      },msg => {
        console.log(msg)
      })
    }
  }
}
</script>

<template>
  <div>
    <button @click="recStart"> 开始录音 </button>
    <br/>
    <br/>
    <button @click="recStop"> 结束录音 </button>
  </div>
</template>

<style scoped>

</style>
