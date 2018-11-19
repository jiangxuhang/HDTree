<template>
  <div>
    <wxParse :content="article" @preview="preview" @navigate="navigate" />
    <button  class="btn-praise btn">点赞</button>
    <button  open-type='share' class='btn-share btn' >转发</button>
    <div class="div_input">
      <div class="float">
        <img class="icon-user" src="../../images/comment.png" mode="aspectFit"/>
      </div>
      <div class="float">
        <input :maxlength="100" placeholder="写评论..." class="input"/>
      </div>          
    </div>
    <div class="commentflex">
    <div class="comment" v-for='(comment,index) in comments' :key="index">
      <div class="float comment_img">
        <img :src="comment.userimage" mode="aspectFit" class="avatar">
      </div>
      <div class="float comment_info column">
        <div class="comment_info_username ">
          {{comment.username}}
        </div>
        <div class="comment_info_">
          {{comment.commentinfo}}
        </div>
        <div class="coment_info_time">
          {{comment.time}}
        </div>
      </div>
      <div class="float comment_praise">

      </div>
    </div>
    </div>
  </div>
</template>

<script>
import wxParse from 'mpvue-wxparse'
export default {
  components: {
    wxParse
  },
  data () {
    return {
      article: '<div>请稍等</div>',
      mid:'',
      id:1,
      comments:[{
        userimage:'../../images/userimg1.jpg',
        username:'极奏',
        commentinfo:'nice',
        parise:'50',
        time:'9小时前'
      },
      {
        userimage:'../../images/userimg2.jpg',
        username:'jizou',
        commentinfo:'nice兄弟nice',
        parise:'2',
        time:'1小时前'
      },
      ]
    }
  },
  methods: {
    preview(src, e) {
      // do something
    },
    navigate(href, e) {
      // do something
    },
    async loading(){
      this.id = this.$root.$mp.query.id
      console.log('id',this.id)
      let self=this;
      // if(this.article=='<div>请稍等</div>')
      // {
      //   this.flag=0;
      // }
      await wx.request({
        url:'https://www.sayetuan.com/XZ/jobhtml?id='+this.id,
        success:function(res){         
          self.article=res.data.html;
      }
    })
    }
  },
  onLoad(){
    this.article='<div>请稍等</div>';
  },
  mounted() {
    this.loading();
  },
}
</script>

<style>
@import url("~mpvue-wxparse/src/wxParse.css");
/* @import url("../../../static/css/spinners.css"); */
.btn{
  display: inline-block;
  border-style:solid;
  width:220rpx;
  height:74rpx;
  background:#ffffff;
  border-radius: 40rpx;
  border-width: 2rpx;
  color: #5C5D5F;
  border-color:#f0f0f0;
  margin-left: 10px;
  font-size: 15px;
}
.div_input{
  width:600rpx;
  height: 64rpx;
  background-color:#f4f5f7;
  border:2px;
  border-radius: 40rpx;
  margin-left: 10px;
  margin-top:20px;
}
.input
{
  width: 364rpx;
  height:64rpx;
  border-style: none;
  font-size:13px;
  padding:5rpx;
}
.icon-user{
  width: 44rpx;
  height: 44rpx;
  padding: 5px;
}
.float{
  display: inline-flex;
}
.commentflex{
  margin-top: 20px;
}
.comment{
  width:100%;
  height:280rpx;
  display:flex;
}
.comment_img{
  width:15%;
  height: 100%;
}
.avatar{
  width:55px;
  height:55px;
  border-radius:50%;
}
.comment_info{
  width:65%;
  height: 100%;
}
.comment_praise{
  background:blue;
  width:20%;
  height:100%;
}
.column{
  flex-direction: column;
}
</style>