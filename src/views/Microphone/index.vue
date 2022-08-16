<template>
  <div>
    <div class="container">
      <audio
        ref="myAudio"
        controls
        autoPlay
        playsInline
      ></audio>
    </div>
    <button @click="openCamera">开始</button>
    <!-- <input ref="myVideo" /> -->
  </div>
</template>

<script lang="ts">
import { defineComponent, getCurrentInstance, onMounted, ref } from "vue";

export default defineComponent({
  setup(props, context) {
    let _window = window as any;
    let instance: any = null;

    //约束条件 禁用音频 启用视频
    const constraints = (_window.constraints = {
      audio: true,
      video: false,
    });

    const openCamera = async () => {
      try {
        let stream = await navigator.mediaDevices.getUserMedia(constraints);
        handleSuccess(stream);
      } catch (error) {
        console.log(error);
        handleError(error);
      }
    };

    const handleSuccess = (stream: any) => {
      const audio = instance.refs.myAudio;
      const audioTracks = stream.getAudioTracks(); // 音频轨道
      console.log("通过设置限制条件获取到流:", constraints);
      console.log(`使用的视频设备: ${audioTracks[0].label}`);
      //使得浏览器能访问到stream
      _window.stream = stream;
      audio.srcObject = stream;
    };

    const handleError = (error: any) => {
        console.log("error")
    };

    onMounted(() => {
      instance = getCurrentInstance();
    });

    return {
      openCamera,
    };
  },
});
</script>

<style scoped>
.container {
  width: 300px;
  height: 300px;
  /* background: #ccc; */
}
</style>
