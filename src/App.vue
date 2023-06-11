<template>
  <div>123212</div>
  <button>dshashcs</button>
  <preview-img >
    <div>
      <img :key="123" src="./assets/bl-hei.png" alt="">
      <img src="./assets/bl-bai.png" alt="">
      <img src="./assets/bl-jin.png" alt="">
    </div>
  </preview-img>
</template>

<script>
import {defineComponent} from "vue"
// const path=require("path")
import previewImg from './packages/preview/preview-img/preview-img.vue';
export default defineComponent({
  name: 'App',
  components:{
    previewImg
  },
  setup(){
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
