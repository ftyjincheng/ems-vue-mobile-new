<template>
  <div class="content">
    <div class="title">邮寄业务办理须知</div>
    <div class="box"><div class="tui" @click="tui()"></div>邮寄业务办理须知</div>
    <div class="txt">
     <div v-html="postInfo"></div>
    </div>
    <!-- <div class="tab">
        <div class="top">
            <input type="checkbox" @click="xuan">
            <div>我已阅读《邮寄业务办理须知》</div>
        </div>
        <button v-bind:disabled="mybtn" @click="jump">同意并进入</button>
    </div> -->
  </div>
</template>
<script>
export default {
    data(){
        return{
          mybtn:true,
          postInfo: ''
        }
    },
    methods:{
        xuan(e){
            var as = e.target.checked;
            this.mybtn=!as
        },
        jump(){
            this.$router.push("/orderInfo")
        },
      getPostInfo(){
        this.$http({
          url: this.$http.adornUrl('/mobile/post/list'),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({ data }) => {
          if (data && data.code === 0) {
            this.postInfo = data.data.postComment
          } else {
            alert(data.msg)
          }
        })
      },
      tui(){
        this.$router.go(-1);
      }
    },
  created(){
      this.getPostInfo()
  }
}
</script>
<style scoped>
body{
    width:100%;
}
.title{
    font-size:0.36rem;
    color:#fff;
    text-align: center;
    background:#213980;
    height:7vh;
    line-height: 7vh;
}
.box{
    width:7.5rem;
    height:7vh;
    font-size: 0.28rem;
    color:#fff;
    line-height: 7vh;
    background:#177abf;
    text-align: center;
    position:relative;
}
.tui{
  position: absolute;
  top:0.33rem;
  left:0.3rem;
  width:0.3rem;
  height:0.3rem;
  background:url("../../img/zuo.png");
  background-size:0.3rem 0.3rem;
  background-repeat: no-repeat;
}
.sj{
    border-top:0.17rem solid #fff;
    border-left:0.13rem solid transparent;
    border-right:0.13rem solid transparent;
    border-bottom:0.13rem solid transparent;
    position:absolute;
    top:2.5vh;
    right:0.4rem;
}
.txt{
    width:7.5rem;
    font-size:0.28rem;
    color:#333;
    height:86vh;
    overflow: auto;
    padding:0.6rem 0.25rem;
    box-sizing: border-box;
}
.txt::-webkit-scrollbar{
    display: none;
}
.tab{
    height:20vh;
    box-sizing: border-box;
}
.top{
    display: flex;
    font-size:0.28rem;
    color:#999;
    padding:0.4rem;
}
.top>input{
    margin-right:0.2rem;
    outline: none;
}
.tab button{
    border:0;
    background:#177abf;
    width:6rem;
    height:1rem;
    color:#fff;
    font-size:0.28rem;
    font-weight: bold;
    margin:0 0.75rem;
    border-radius: 5px;
    display: block;
}
.tab button[disabled]{
    background-color: #b1c5dc;
}
</style>
