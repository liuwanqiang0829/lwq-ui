<template>
  <div>
    <div @click="show" ref="imgRef">
      <slot></slot>
    </div>
    <div class="preview-img-container" v-if="showImg">
      <div class="close" @click="close">
        <div class="close-icon"></div>
      </div>
       <div class="title" v-if="title"></div>
       <div class="left-icon" v-if="imgList.length>1">
        <button 
        class="left-btn"
        @click="preShowImg"
        :disabled="currentIndex===0"
        ></button>
       </div>
       <div 
       class="right-icon" 
       v-if="imgList.length > 1"
       >
        <button class="right-btn"
          @click="nextShowImg"
          :disabled="currentIndex===imgList.length-1"
        ></button>
       </div>
       <div class="img-box">
        <img :src="currentImg" :style="imgClass"  @mousewheel="watchCursorWheel($event)" alt="">
       </div>
    </div>
  </div>
</template>
<script>
import {onMounted, reactive, ref} from "vue"
export default {
  name:"PreviewImg",
  setup(props,context){
    let showImg=ref(false)
    let currentImg=ref("")
    let imgClass=reactive({})
    let currentIndex=ref(0)
    let imgRef=ref(null)//拿取插槽中的内容
    let title=ref("")
    let imgList=reactive([])
    onMounted(()=>{
      console.log("dom",imgRef)
      imgList.push(...getImgElement(imgRef.value.children))
    })
    //点击图片预览
    function show(e){
      currentImg.value=e.target.src
      showImg.value=true
      title.value="logo"
      currentIndex.value=imgList.indexOf(currentImg.value)
      
     }
    //递归遍历插槽中的图片元素
    function getImgElement(slot){
      let arr=[]
        for(let i=0;i<slot.length;i++){
           if(slot[i].localName==="img"&&slot[i].src){
            arr.push(slot[i].src)
           }
           if(slot[i].children&&slot[i].children.length>0){
            arr=arr.concat(getImgElement(slot[i].children))
           }
        }
        return arr
    }
    function nextShowImg(){
      console.log("imgList",imgList,currentImg,currentIndex)
      currentImg.value=imgList[++currentIndex.value]
    }
    function preShowImg(){
      currentImg.value=imgList[--currentIndex.value]
    }
    //监视鼠标滚动事件
     function watchCursorWheel(event){   
      const width=event.target.clientWidth
      const height=event.target.clientHeight
      if(event.wheelDeltaY>0){
        imgClass.width=width*1.2+"px"
        imgClass.height=height*1.2+"px"
      }else{
        imgClass.width=width*0.8+"px"
        imgClass.height=height*0.8+"px"
      }
    }
    
      //关闭弹窗
      function close(){
       showImg.value=false

    }
    return{
      showImg,
      currentImg,
      currentIndex,
      imgList,
      imgClass,
      imgRef,
      close,
      show,
      nextShowImg,
      preShowImg,
      watchCursorWheel
    }
  }
}
</script>
<style lang="less" scoped>
.preview-img-container{
  position: fixed;
  left: 0;
  top: 0;
  z-index: 99;
  right: 0;
  bottom:0;
  &:hover{
    cursor: pointer;
  }
  background-color: rgba(0,0,0,0.9);
  img{
    position: absolute;
    left: 50%;
    right:50%;
    top: 50%;
    transform: translate(-50%,calc(-50% - 20px));
    max-width: 80%;
    max-height: 80%;
    min-width: 20%;
    min-height: 20%;
    &:hover{
      cursor: crosshair;
    }
    object-fit: contain;
  }
  .left-icon{
    position: absolute;
    left: 5%;
    top: 50%;
    transform: translateX(-50%);
    width: 50px;
    height: 50px;
    border-radius: 25px;
    background-color: rgba(0, 0, 0, 1);
    display: flex;
    justify-content: center;
    align-items: center;
    .left-btn{
      width: 10px;
      height: 15px;
      margin-left: 5px;
      background-color: transparent;
      border-color: transparent;
      transform: rotate(45deg);
      border-left: 2px solid #ffffff;
      border-bottom: 2px solid #ffffff;
    }

  }
  .right-icon{
    position: absolute;
    right: 2%;
    top: 50%;
    transform: translateX(-50%);
    width: 50px;
    height: 50px;
    z-index: 10;
    border-radius: 25px;
    background-color: rgba(0, 0, 0, 1);
    display: flex;
    justify-content: center;
    align-items: center;
    .right-btn{
      width: 10px;
      height: 15px;
      margin-left: -5px;
      background-color: transparent;
      border-color: transparent;
      transform: rotate(45deg);
      border-top: 2px solid #ffffff;
      border-right: 2px solid #ffffff;
    }

  }
}
.title{
  width: 100%;
  height: 30px;
  line-height: 30px;
  text-align: center;
  background-color: rgba(219, 208, 181, 1);
}
.close{
  position: absolute;
  top:10px;
  right: 25px;
  width: 50px;
  height: 50px;
  background-color: #fff9f9;
  border-radius: 25px;
}
.close-icon::before,.close-icon::after{
  position: absolute;
  top:16px;
  right: 25px;
  content: "";
  width: 2px;
  height: 20px;
  background-color: #141111;

}
.close-icon::before{
  transform: rotate(45deg);
}
.close-icon::after{
  transform: rotate(-45deg);
}
</style>
