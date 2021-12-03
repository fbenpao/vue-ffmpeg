<template>
  <video controls ref="videoPlayer" width="320" height="240">
    <source :src="videoUrl" type="video/mp4">
</video>
  <input type="file" @change="filechange($event)" accept="video/*">
     <div class="home">
      <button @click="startRecord()">开始录音</button>
      <button @click="continuRecord()">继续录音</button>
      <button @click="stopRecord()">结束录音</button>
      <button @click="playRecord()">录音播放</button>
    </div>
</template>

<script>
import Recorder from 'js-audio-recorder';
let recorder = new Recorder();
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      count: 0,
      videoUrl:""
    }
  },
  mounted() {
    //获取麦克风权限
    Recorder.getPermission().then(() => {
    console.log('给权限了');
    }, (error) => {
        console.log(`${error.name} : ${error.message}`);
    });
},
  methods:{
    startRecord () {
      console.log(1)
      // this.showList()
       recorder.start().then(() => {
        // 开始录音
        console.log("开始录音")
        }, (error) => {
            // 出错了
            console.log(`${error.name} : ${error.message}`);
        });
    },
    continuRecord () {
      console.log(2)
      recorder.resume()// 继续录音
    },
    stopRecord () {
      console.log(3)
      recorder.stop() // 结束录音
      console.log("录音时长:",recorder.duration)
      console.log("音频大小:",recorder.fileSize);
    },
    playRecord () {
      console.log(4)
      recorder.play() // 录音播放
    },
   filechange(e){   
     console.log(e)
   const resultFile = e.target.files;
   console.log(resultFile[0])
   const aBlob = new Blob([resultFile[0]],{type:'video/mp4'})
   console.log(aBlob)
   const reader = new FileReader();
   reader.onload=(ev)=>{
     this.videoUrl = ev.target.result
     this.$refs.videoPlayer.load()
   }
   reader.readAsDataURL(aBlob)
  
 },
 
  }
}
</script>
