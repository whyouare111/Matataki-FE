<template>
  <div class="tg-container">
    <!-- <div v-loading='loading' style='height: 100%;' element-loading-text='登录中...' /> -->
    <p class="tips">
      loading...
    </p>
    <p class="tips">
      使用该功能需要“科学上网”
    </p>
  </div>
</template>

<script>
export default {
  layout: 'empty',
  data() {
    return {
      token: {}
    }
  },
  async mounted() {
    const response = await this.$API.twitterLoginPrepare(this.$route.query.from)

    window.location.href = 'https://api.twitter.com/oauth/authenticate?oauth_token=' + response.data
  },
  methods: {
  }
}
</script>

<style scoped lang='less'>
.tg-container {
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  padding-top: 10%;
  .tips {
    font-size: 14px;
    color: #333;
    padding: 0;
    margin: 2em 0 0;
  }
}
</style>
