<template>
  <div>
    <div class="container">
      <video
        style="width: 30px; height: 30px"
        ref="myVideo"
        autoPlay
        playsInline
      ></video>
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
      audio: false,
      video: true,
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
      const video = instance.refs.myVideo;
      const videoTracks = stream.getVideoTracks(); // 视频轨道
      console.log("通过设置限制条件获取到流:", constraints);
      console.log(`使用的视频设备: ${videoTracks[0].label}`);
      //使得浏览器能访问到stream
      _window.stream = stream;
      video.srcObject = stream;
    };

    const handleError = (error: any) => {
    //   if (error.name === "ConstraintNotSatisfiedError") {
    //     const v = constraints.video;
    //     //宽高尺寸错误
    //     message.error(`宽:${v.width.exact} 高:${v.height.exact} 设备不支持`);
    //   } else if (error.name === "PermissionDeniedError") {
    //     message.error("没有摄像头和麦克风使用权限,请点击允许按钮");
    //   }
    //   message.error(`getUserMedia错误: ${error.name}`, error);
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
  background: #ccc;
}
</style>
