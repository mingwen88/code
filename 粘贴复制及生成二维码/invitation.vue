<template>
  <div>
    <mt-header title="邀请推广" class="head" fixed>
      <router-link to="/mine" slot="left">
        <div>
          <mt-button icon="back" class="back_con"></mt-button>
        </div>
      </router-link>
    </mt-header>

    <div class="invitation">
      <div class="invitcode">
        <div class="title">您的邀请二维码</div>
        <div>
          <VueQrcode :value="invite_link" :options="{ size: 240 }" style="height: auto" class="charge-code-img1"></VueQrcode>
        </div>
        <div class="code">
          <span>邀请码：{{invite_link}}</span>
        </div>
        <button class="btn" v-clipboard:copy="invite_link" v-clipboard:success="onCopy" v-clipboard:error="onError">点击分享</button>
        <div class="tis">点击分享即可保存链接</div>
      </div>

      <div class="sign sign1">
        <span class="math">1</span>
        <span class="content">点击复制链接邀请好友注册，朋友扫码加入</span>
      </div>
      <div class="sign">
        <span class="math">2</span>
        <span class="content">朋友成功注册后，即可领取丰厚奖励</span>
      </div>
    </div>
  </div>
</template>

<script>
import VueQrcode from '@xkeshi/vue-qrcode'
export default {
  components: {
    VueQrcode
  },
  data () {
    return {
      invite_link: ''
    }
  },
  methods: {
    onCopy () {
      this.$layer.msg('复制成功')
    },
    onError () {
      this.$layer.msg('复制失败')
    },
    getInfo () {
      this.$axios({
        url:this.global.path + '/api/My/invitationPromotion',
        // url: 'apis/api/My/invitationPromotion',
        method: 'post',
        headers: {
          token: localStorage.token
        }
      }).then(res => {
        var res = this.Decrypt(res.data.receive)
        res = JSON.parse(res)
        console.log(res)
        if (res.code == 200) {
          this.invite_link = this.global.path + '#/register?id=' + res.data.uid
        } else if (res.code == 300) {
          this.$layer.msg(res.message)
          localStorage.clear()
          this.$router.push('/login')
        }
      })
    }
  },
  mounted () {
    this.getInfo()
  }
}
</script>

<style scoped lang="scss">
  .invitation{
    height: 100%;
    min-height: 100vh;
    overflow: hidden;
    margin-top: 1.4rem;
    background: linear-gradient(to bottom,rgba(62, 10, 148, 0.6),rgba(171, 2, 109, 0.6));
    .invitcode{
      width: 80%;
      margin-left: 10%;
      background-color: #fff;
      margin-top: 90px;
      border-radius: 20px;
      .title{
        line-height: 200px;
        font-size: 30px;
        font-weight: 600;
      }
      .code{
        line-height: 140px;
        font-size: 28px;
      }
      .btn{
        width: 80%;

        line-height: 90px;
        background: linear-gradient(to bottom,rgba(154, 9, 118, 1),rgba(108, 13, 135, 1));
        font-size: 28px;
        color: #fff;
        border-radius: 45px;
        margin-top: 10px;
      }
      .tis{
        line-height: 80px;
        font-size: 24px;
        color: rgba(153, 153, 153, 1);
        margin-bottom: 60px;
      }
    }
    .sign{
      width: 90%;
      margin-left: 5%;
      text-align: left;
      line-height: 60px;
      color: #fff;
      font-size: 24px;
      .math{
        height: 35px;
        line-height: 35px;
        width: 35px;
        border-radius: 50%;
        display: inline-block;
        background-color: #ccc;
        text-align: center;
        color: #000;
        margin-right: 10px;
      }
    }
    .sign1{
      margin-top: 80px;
    }
  }
</style>
