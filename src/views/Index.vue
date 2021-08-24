<template>
  <div class="home">
    <van-field v-model="originProductUrl"
               rows="3"
               autosize
               label="原商品地址"
               type="textarea"
               placeholder="请输入原商品地址" />
    <van-button type="primary"
                size="small"
                @click="genShopCarUrl">生成下单地址</van-button>
    <van-field v-model="resultUrl"
               rows="3"
               autosize
               label="生成地址"
               type="textarea" />
    <van-button type="info"
                size="small"
                v-clipboard:copy="resultUrl"
                v-clipboard:success="onCopy"
                v-clipboard:error="onCopyError">复制地址</van-button>
    <van-button type="info"
                size="small"
                style="margin-left:30px"
                @click="toTargetUrl">跳转</van-button>
  </div>
</template>

<script>
import Vue from 'vue'
import { Field, Button, Toast } from 'vant'
import VueClipboard from 'vue-clipboard2'
Vue.use(Field).use(Button).use(VueClipboard).use(Toast)

export default {
  name: 'Index',
  data() {
    return {
      originProductUrl: '',
      resultUrl: '',
    }
  },
  methods: {
    genShopCarUrl() {
      if (this.isEmpty(this.originProductUrl)) {
        Toast('请输入原商品地址')
        return false
      }
      const arr = this.originProductUrl.split('.html')
      if (arr.length <= 1) {
        Toast('请输入正确的原商品地址')
        return false
      }
      const arr1 = arr[0].split('https://item.m.jd.com/product/')
      if (arr1.length <= 1) {
        Toast('请输入正确的原商品地址')
        return false
      }
      const productId = arr1[1]
      this.resultUrl =
        'https://wqdeal.jd.com/deal/confirmorder/main?commlist=' +
        productId +
        ',,1,' +
        productId +
        ',1,0,0#wechat_redirect'
    },
    onCopy() {
      Toast.success('复制成功')
    },
    onCopyError() {
      Toast.fail('复制失败')
    },
    toTargetUrl() {
      var a = document.createElement('a')
      a.setAttribute('href', this.resultUrl)
      a.setAttribute('target', '_blank')
      a.click()
    },
    isEmpty(str) {
      if (str == '' || str == 'undefined') {
        return true
      } else {
        return false
      }
    },
  },
}
</script>
