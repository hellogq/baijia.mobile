<template>
  <div class="select">
    <div class="banner">
      <div class="box">
        <img v-for='img in imgs' class="img" :src="setBannerSrc(img.src)"/>
      </div>
      <ul class="num">
      	<li v-for='i in 4'></li>
      </ul>
    </div>
    <section class="news">
      <div v-if='newsDate.lenght'>
        <div v-for='n in newsDate'>
          <a href="#" class="new">
            <img :src="setNewSrc(n.imageurls[0].url)"/>
            <div class="intro">
              <h4>{{n.title}}</h4>
              <p><span>{{n.source}}</span> | <span>{{n.pubDate}}</span></p>
            </div>
          </a>
        </div>
      </div>
      <div class="fail" v-else>~~~~(>_<)~~~~， 请求到数据失败!</div>
    </section>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: 'select',
  data () {
    return {
      imgs: [{
        src: require('../assets/select/a.jpg')
      }, {
        src: require('../assets/select/b.jpg')
      }, {
        src: require('../assets/select/c.jpg')
      }, {
        src: require('../assets/select/d.jpg')
      }],
      newsDate: []
    }
  },
  created: function () {
    let self = this
    let newsUrl = 'https://route.showapi.com/109-35?page=1&needContent=0&needHtml=1&showapi_appid=34477&showapi_sign=cfa5957a730f43d38886bd16469b2a86&channelId=5572a108b3cdc86cf39001cd'
    requestData(newsUrl)
    function requestData (url) {
      $.ajax({
        type: 'get',
        url: url,
        async: true,
        success: function (res) {
          let data = res.showapi_res_body.pagebean.contentlist
          for (var i = 0; i < data.length; i++) {
            if (data[i].imageurls[0]) {
              self.newsDate.push(data[i])
            }
          }
        },
        error: function (err) {
          console.log(err.statusText)
        }
      })
    }
    console.log('叩首为梦 码梦为生！')
  },
  mounted () {
    let num = 0
    let timer = null
    let time = 3000
    let imgs = document.querySelectorAll('.select .box img')
    let nums = document.querySelectorAll('.select .num li')
    let len = imgs.length - 1
    function carousel () {
      num === len ? num = 0 : num++
      changeImg(imgs, nums, num)
      clickShow()
    }
    $('.banner').hover(stop, play)
    // 点击切换图片
    function clickShow () {
      for (let i = 0; i < nums.length; i++) {
        nums[i].addEventListener('click', function (e) {
          let activeIndex = getIndex(nums, e.target)
          changeImg(imgs, nums, activeIndex)
        })
      }
    }
    // 添加动画
    function play () {
      timer = setInterval(function () {
        carousel()
      }, time)
    }
    // 清除动画
    function stop () {
      clearInterval(timer)
    }
    // 切换图片
    function changeImg (arr1, arr2, num) {
      for (let i = 0; i < arr1.length; i++) {
        if (num === i) {
          $(arr1[i]).show()
          arr2[i].className = 'active'
        } else {
          $(arr1[i]).hide()
          arr2[i].className = ''
        }
      }
    }
    // 得到特定对象下标
    function getIndex (arr, obj) {
      return [].indexOf.call(arr, obj)
    }
    play()
  },
  methods: {
    setBannerSrc (src) {
      return src
    },
    setNewSrc (url) {
      return url
    },
    removeTimer () {
      clearInterval(this.timer)
    }
  }
}
</script>

<style lang="stylus">
.select{
  background: #fff;
  .banner{
    position: relative;
    width: 100%;
    height: 200px;
    overflow: hidden;
    .box{
      height: 200px;
      width: 100%;
      position: absolute;
      left: 0;
      top: 0;
      display: flex;
      justify-content: flex-start;
    }
    .img{
      height: 200px;
      width: 100%;
      transition: all 1s;
    }
    .num{
      display: flex;
      list-style: none;
      position: absolute;
      bottom: 10px;
      right: 10px;
      .active{
        background: #058 !important;
      }
      li{
        height: 10px;
        width: 10px;
        cursor: pointer;
        background: #fff;
        border-radius: 50%;
        margin: 10px;
      }
    }
  }
  .news{
    padding: 0 10px;
    .new{
      height: 100px;
      color: #262627;
      border-bottom: 1px solid #eee;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      width: 100%;
      img{
        height: 80px;
        width: 100px;
      }
      .intro{
        width: 80%;
        height: 80px;
        display: flex;
        padding-left: 10px;
        flex-direction: column;
        justify-content: space-between;
        h4{
          font-size: 20px;
          line-height: 1.2;
          font-weight: bold;
          overflow: hidden;
          display: -webkit-box;
          word-break: break-all;
          -webkit-line-clamp: 2;
          text-overflow: ellipsis;
          -webkit-box-orient: vertical;

        }
        p{
          font-size: 13px;
          color: #666;
        }
      }
    }
    .fail{
      min-height: 300px;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
</style>
