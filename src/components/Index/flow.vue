<template>
  <div class="content">
    <div class="title">不动产登记便民邮寄</div>
    <img src="../../img/beijing.png" class="bj">
    <div class="box">不动产权证办理流程</div>
    <div class="box1">
      <div class="item1">
        <div class="fang1">
          <div class="yuan1">1</div>
        </div>
        <div class="ft1">提交申请</div>
      </div>
      <div class="item2">
        <div class="ft2">业务费用支付</div>
        <div class="fang2">
          <div class="yuan2">2</div>
        </div>
      </div>
      <div class="item1">
        <div class="fang1">
          <div class="yuan1">3</div>
        </div>
        <div class="ft1">不动产权证交接</div>
      </div>
      <div class="item2">
        <div class="ft2">快递员派送</div>
        <div class="fang2">
          <div class="yuan2">4</div>
        </div>
      </div>
      <div class="item1">
        <div class="fang1">
          <div class="yuan1">5</div>
        </div>
        <div class="ft1">业务完成</div>
      </div>
    </div>
    <!--  受理人信息确认弹出框  -->
    <div class='fuceng' v-if="sureInfo">
      <div class='fuceng_tip'>
        <div class='fuceng_title'>提醒</div>
        <div class="info_list">{{dialogContent}}！</div>
        <div class='btn_list'>
          <!-- <div class='xiugai' @click="xiugai">修改</div> -->
          <div class='queding' @click="sureNext">确定</div>
        </div>
      </div>
    </div>
    <div class="agree">
      <input type="checkbox" class='is_agree' v-model="checked">
      <p>
        <span class='meiyongde'>同意</span>
        <router-link to="/contract" class='bind_tap'>《邮寄业务办理须知》</router-link>
      </p>
    </div>
    <div class="tab">
      <div @click="tip">快速办理>></div>
      <div @click="list">查询进度>></div>
      <img src="../../img/kdy.png">
    </div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        openid: '',
        checked: false,
        sureInfo: false,
        dialogContent: ''
      }
    },
    methods: {
      sureNext() {
        this.sureInfo = false;
        if(this.checked)
        {
          this.$router.push({
            name: 'firstOrder'
          })
        }
      },
      showTip() {
        this.dialogContent = "办理完毕后务必将受理凭证交到不动产大厅邮政窗口！"
        this.sureInfo = true
      },
      tip() {
        if (this.checked) {
          this.dialogContent = "办理完毕后务必将受理凭证交到不动产大厅邮政窗口！"
          this.sureInfo = true
          // this.$router.replace('/orderInfo')
        } else {
          this.dialogContent = "请先同意不动产权证邮寄业务办理须知！"
          this.sureInfo = true
          // alert("请先同意不动产权证邮寄业务办理须知")
        }
      },
      list() {
        if (this.checked) {
          this.$router.push({
            path: `/list/${localStorage.getItem('openid')}`
          })
        } else {
          this.dialogContent = "请先同意不动产权证邮寄业务办理须知！"
          this.sureInfo = true
          // alert("请先同意不动产权证邮寄业务办理须知")
        }
      },
      validateLogin() {
        this.$http({
          url: this.$http.adornUrl('/wechat/validateLogin'),
          method: 'get',
          params: this.$http.adornParams({
            'openid': this.openid
          })
        }).then(({
          data
        }) => {
          if (data && data.code === 0) {
            console.log("登陆成功")
          } else {
            console.log("授权失败")
          }
        })
      },
      // getQueryString(name){
      //   let reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)', 'i');
      //   let r = window.location.search.substr(1).match(reg);
      //   if (r != null) {
      //     return unescape(r[2]);
      //   }
      //   return null;
      // },
      // getPostNotify(){
      //   this.$http({
      //     url: this.$http.adornUrl('/mobile/order/notify'),
      //     method: 'get',
      //     params: this.$http.adornParams()
      //   }).then(({ data }) => {
      //     if (data && data.code === 0) {
      //       console.log(data)
      //     }
      //   })
      // },
    },
    created() {
      // alert("获取页面链接地址")
      // alert(window.location.href)

      let webAddress = window.location.href
      localStorage.setItem("openid", webAddress.split("/")[webAddress.split("/").length - 2])
      localStorage.setItem("areaid", webAddress.split("/")[webAddress.split("/").length - 1])

      console.log("openid===" + localStorage.getItem('openid'))
      console.log("areaid===" + localStorage.getItem('areaid'))

      // alert("openid=====" + localStorage.getItem("loginOpen"))
      // alert(localStorage.getItem("areaid"))

      // this.getPostNotify()
      this.openid = localStorage.getItem("openid")
      if (this.openid == "null" || this.openid == null || this.openid == "") {
        // // let parametertext = this.getQueryString('openid');  // => ‘参数内容’
        // // localStorage.setItem("openid",parametertext);
        // let webAddress = window.location.href
        // localStorage.setItem("openid", webAddress.split("/")[webAddress.split("/").length - 2])
        // localStorage.setItem("areaid", webAddress.split("/")[webAddress.split("/").length - 1])
        //
        // console.log(localStorage.getItem('openid'))
        // console.log(localStorage.getItem('areaid'))
        this.openid = localStorage.getItem("openid")
      }
      this.$cookie.set('token', this.openid)
      this.validateLogin()
    }
  }
</script>
<style scoped>
  .tab div {
    width: 50%;
    height: 100%;
    text-align: center;
  }

  .bind_tap {
    color: #177abf !important;
    text-decoration-line: none;
  }

  .agree {
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }

  .is_agree {
    margin-right: 0.2rem;
    width: 0.7rem;
    height: 0.7rem;
    margin-left: 0.4rem;
  }

  .meiyongde {
    color: #333333 !important;
  }

  .agree {
    font-size: 0.28rem;
  }

  body {
    width: 100%;
  }

  .content {
    font-size: 0.28rem;
  }

  .title {
    font-size: 0.36rem;
    color: #fff;
    text-align: center;
    background: #213980;
    height: 0.8rem;
    line-height: 0.8rem;
  }

  .bj {
    display: block;
    width: 7.5rem;
  }

  .box {
    width: 7.1rem;
    height: 0.8rem;
    margin: 0.2rem;
    color: #fff;
    line-height: 0.8rem;
    background: #177abf;
    border-radius: 5px;
    text-align: center;
    position: relative;
  }

  .box1 {
    padding: 0.1rem 0 0;
  }

  .item1 {
    display: flex;
    align-items: center;
    padding-left: 2.9rem;
    margin-bottom: 0.4rem;
  }

  .item2 {
    display: flex;
    align-items: center;
    padding-right: 3.2rem;
    justify-content: flex-end;
    margin-bottom: 0.6rem;
  }

  .fang1 {
    background: #468cce;
    width: 0.9rem;
    height: 0.9rem;
    font-size: 0.4rem;
    transform: rotate(45deg);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .fang2 {
    background: #0156a7;
    width: 0.9rem;
    height: 0.9rem;
    font-size: 0.4rem;
    transform: rotate(45deg);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .yuan1 {
    background: #fff;
    text-align: center;
    width: 0.8rem;
    height: 0.8rem;
    line-height: 0.8rem;
    border-radius: 50%;
    transform: rotate(-45deg);
    color: #468cce;
  }

  .yuan2 {
    background: #fff;
    text-align: center;
    width: 0.8rem;
    height: 0.8rem;
    line-height: 0.8rem;
    border-radius: 50%;
    transform: rotate(-45deg);
    color: #0156a7;
  }

  .ft1 {
    margin-left: -0.45rem;
    height: 0.8rem;
    background: #f0f0f0;
    border-top-right-radius: 50px;
    border-bottom-right-radius: 50px;
    padding: 0 0.6rem 0 0.9rem;
    line-height: 0.8rem;
    font-size: 0.28rem;
    color: #666;
    box-shadow: 0px 0.05rem 10px 0px #c6cbd6;
  }

  .ft2 {
    margin-right: -0.45rem;
    height: 0.8rem;
    background: #f0f0f0;
    border-top-left-radius: 50px;
    border-bottom-left-radius: 50px;
    padding: 0 0.9rem 0 0.6rem;
    line-height: 0.8rem;
    font-size: 0.28rem;
    color: #666;
    box-shadow: 0px 0.05rem 10px 0px #c6cbd6;
  }

  .tab {
    background: #383735;
    color: #fff;
    font-size: 0.36rem;
    height: 1rem;
    line-height: 1rem;
    font-weight: bold;
    position: relative;
    display: flex;
  }

  .tab>img {
    display: none;
    position: absolute;
    width: 2rem;
    right: 0.25rem;
    bottom: 0;
  }

  .tab>a {
    text-decoration: none;
    display: block;
    color: #fff;
    width: 3.75rem;
    text-align: center;
  }

  .fuceng {
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .5);
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    z-index: 333;
  }

  .fuceng_tip {
    font-size: 0.28rem;
    width: 5.45rem;
    height: 3.5rem;
    position: fixed;
    border-radius: 0.2rem;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    background: white;
    z-index: 666;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    flex-direction: column;
  }

  .fuceng_title {
    font-size: 0.36rem !important;
    color: #333;
    font-weight: bold;
    margin-top: 0.5rem;
  }

  .btn_list {
    width: 100%;
    height: 0.9rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 0.35rem;
    border-top: 1px solid #dedede;

  }

  .btn_list .xiugai {
    border-right: 1px solid #dedede;
  }

  .btn_list .queding {
    color: #09bb07;
  }

  .btn_list div {
    font-size: 0.36rem;
    width: 100%;
    height: 100%;
    text-align: center;
    line-height: 0.9rem;
  }

  .info_list {
    margin-top: 0.45rem;
    margin-left: 0.5rem;
    margin-right: 0.5rem;
  }

  .info_list div {
    margin: 0.1rem 0.5rem;
  }
</style>
