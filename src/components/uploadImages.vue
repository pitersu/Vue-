<template>
  <div class="avatar">
    <div class="hasPic" v-for="(item,index) in imgUrls" :key="index">
      <img
        class="seledPic"
        ref="picture"
        :src="item?item:require('../assets/imagebj.jpg')"
        name="avatar"
        @click="bigImg(index)"
      />
      <img class="delect" src="../assets/del.png" @click="delect(index)" />
    </div>
    <div class="imgMask" v-show="showBigImg" @click.stop="showBigImg=!showBigImg">
      <div class="showImg">
        <div class="swiper-container" ref="clickSwiper">
          <div class="swiper-wrapper">
            <!-- <div class="swiper-slide num" v-for="(item,index) in imgUrls" :key="index">
              <img :src="imgUrls[index]" class="img" />
            </div>-->
          </div>
          <!-- 如果需要分页器 -->
          <div class="swiper-pagination"></div>
        </div>
      </div>
    </div>
    <div class="selPic" v-if="imgUrls.length<6">
      <img src="../assets/imagebj.jpg" name="avatar" />
      <span>{{pictureNums}}</span>
      <input
        type="file"
        maxlength
        class="input-file"
        multiple="multiple"
        name="avatar"
        ref="avatarInput"
        @change="changeImage($event)"
        accept="image/gif, image/jpeg, image/jpg, image/png"
      />
    </div>
  </div>
</template>
<script>
import Swiper from "swiper"
export default {
  name: 'upload',
  props: ['uploadType', 'imgUrl'],
  data () {
    return {
      showBigImg: false,
      maxImages: 6,
      leftImages: 0,
      pictureNums: '上传图片',
      imgUrls: [],
      avatar: '',
      file: '',
      showBg: false,
      mySwiper: null
    }
  },
  created () {
    this.avatar = this.imgUrl
  },
  mounted () {
    this.mySwiper = new Swiper('.swiper-container', {
      loop: true, // 循环模式选项
      // 如果需要分页器
      width: document.documentElement.clientWidth,
      pagination: {
        el: '.swiper-pagination',
        type: 'fraction',
      },
    })
  },
  methods: {
    changeImage (e) {
      for (var i = 0; i < e.target.files.length; i++) {
        let file = e.target.files[i]
        this.file = file
        let reader = new FileReader()
        let that = this
        reader.readAsDataURL(file)
        reader.onload = function (e) {
          that.imgUrls.push(this.result)
        }
      }
      // 剩余张数
      this.leftImages = this.maxImages - (this.imgUrls.length + e.target.files.length)
      this.pictureNums = String(this.maxImages - (this.imgUrls.length + e.target.files.length)) + '/' + String(this.maxImages)

    },
    delect (index) {
      this.imgUrls.splice(index, 1)
      this.leftImages++
      if (this.leftImages === this.maxImages) {
        this.pictureNums = '上传图片'
      } else {
        this.pictureNums = String(this.leftImages) + '/' + String(this.maxImages)
      }
    },
    bigImg (index) {
      let items = []
      this.mySwiper.removeAllSlides()
      this.showBigImg = true

      for (let i = 0; i < this.imgUrls.length; i++) {
        const item = this.imgUrls[i];
        items.push(`<div class="swiper-slide num"><img src="${item}" class="img" /></div>`)
      }
      this.mySwiper.appendSlide(items);
      this.mySwiper.slideTo(index + 1, 0, false)
    }
  }
}
</script>
<style>
.avatar {
  width: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.hasPic {
  position: relative;
  width: 4.5625rem;
  height: 4.5625rem;
  margin-left: 10px;
  margin-bottom: 10px;
}
.input-file {
  width: 4.5625rem;
  height: 4.5625rem;
  background: red;
  opacity: 0;
}
.seledPic {
  align-items: center;
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.delect {
  position: absolute;
  top: -5px;
  right: -2px;
  width: 15px;
  height: 15px;
}
.selPic {
  margin-left: 10px;
  margin-bottom: 10px;
  position: relative;
  width: 4.5625rem;
  height: 4.5625rem;
}
.selPic img {
  position: absolute;
  width: 4.5625rem;
  height: 4.5625rem;
  top: 0px;
  left: 0px;
}
.selPic span {
  position: absolute;
  bottom: 0.9rem;
  width: 4.5625rem;
  color: #c4c3c3;
  font-size: 0.75rem;
  left: 0px;
  text-align: center;
}
.selPic input {
  position: absolute;
  top: 0px;
  left: 0px;
}
.imgMask {
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0px;
  left: 0;
  z-index: 100;
  background: #fff;
}
.num {
  padding-top: 10px;
  color: white;
  font-size: 0.875rem;
  font-weight: bold;
}
.showImg {
  height: 100%;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  /* position: absolute;
    align-items: center;
    left: 0px;
    top:0;
    color: white; */
}
.showImg img {
  width: 100%;
  height: 100%;
  margin: 0 auto;
}
.avatar /deep/ .swiper-pagination {
  position: none;
}
</style>
