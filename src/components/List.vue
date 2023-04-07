<template>
  <div class="container">
    <div class="item" v-for="(item, index) in  list" :style="{
      backgroundColor: item.background
    }">
      {{ index }}
    </div>
    <Loading v-if="loading" />
  </div>
</template>
 
<script>
import Loading from "./Loading.vue";
export default {
  name: 'List',
  components: {
    Loading,
  },
  props: {
  },
  data() {
    return {
      list: [{ background: 'rgb(233,32,38)' }],
      loading: false
    }
  },
  watch: {
    loading(newVal, oldVal) {
      console.log(newVal, oldVal, '999')
    }
  },
  methods: {
    // 随机生成rgb颜色
    rgb() {
      const r = Math.floor(Math.random() * 256);
      const g = Math.floor(Math.random() * 256);
      const b = Math.floor(Math.random() * 256);
      return `rgb(${r},${g},${b})`;
    },
    async getList(length) {
      if (!length) return
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve([...this.list, ...Array(length).fill(true).map(() => ({ background: this.rgb() }))])
        }, 500)
      })
    },
    throttle(handler, wait) {
      let lastTime = 0;
      return function () {
        let nowTime = new Date().getTime();
        if (nowTime - lastTime > wait) {
          handler.apply(this, arguments);
          lastTime = nowTime;
        }
      }
    },
    scrollLoadMore() {
      let scrollWrap = document.querySelector('.container');
      // 超过50不再监听
      if (this.list.length >= 50) return scrollWrap.removeEventListener('scroll', this.scrollLoadMore)
      let currentScrollTop = scrollWrap.scrollTop;
      let currentOffsetHeight = scrollWrap.scrollHeight;
      let currentClientHeight = scrollWrap.clientHeight;
      if ((currentScrollTop + currentClientHeight >= currentOffsetHeight / 2)) {
        if (!this.loading) {
          this.setList()
        }
        this.loading = true
      }
    },

    // 设置列表
    async setList() {
      const newArray = await this.getList(10)
      this.list = newArray
      this.loading = false
    }
  },
  created() {
    this.setList()
  },

  mounted() {
    this.$nextTick(() => {
      if (document.querySelector('.container')) {
        const selectWrap = document.querySelector('.container')
        selectWrap.addEventListener('scroll', this.throttle(this.scrollLoadMore, 800))
      }
    })
  }

}
</script>
<style>
.item {
  width: 100%;
  height: 500px;
  line-height: 500px;
}

.container {
  height: 100vh;
  overflow-y: scroll;
}
</style>
