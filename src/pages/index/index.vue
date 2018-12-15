<template>
  <div class="externallayout">

    <img src="../images/background.png" class="backgroundimg">

    <div @click="refreshscore">
      <img src="../images/userscore.png" class="userscore">
      <div class="scoreinfo">{{scoreinfo}}</div>
    </div>

    <div @click="ifrank">
      <img src="../images/rank.png" alt="" class="rank">
    </div>

    <div>
      <img src="../images/share.png" alt="" class="share">
    </div>

    <div @click="jump_login">
      <img src="../images/home.png" alt="" class="home">
    </div>

    <div @click='jump_ltxz'>
      <img src="../images/ltxz.png" alt="" class="ltxz">
    </div>

    <div @click='jump_kcb'>
      <img src="../images/kcb.png" alt="" class="kcb">
    </div>

    <div  @click="jump_iHD">
      <img src="../images/iHD.png" alt="" class="iHD">
    </div>

    <button class="login" v-if="loginflag" @click="jump_login">请登录</button>

    <button class="share button" open-type='share'></button>

    <div v-if="wxnameflag" class="unlogin">
      <div @click="logincancel">
        <img src="../images/cancel.png" alt="" class="cancelbutton">
      </div>
      <div >
        <img src="../images/confirm.png" alt="" class="comfirmbutton">
      </div>
      <button class="comfirmbutton button" open-type="getUserInfo" @click="wxlogin"></button>
      <img src="../images/ranktips.png" alt="" class="unlogintips">
    </div>


    <div v-if="shareflag" class="unlogin">
      <div @click="sharecancel">
        <img src="../images/cancel.png" alt="" class="cancelbutton">
      </div>
      <img src="../images/unshare.png" alt="" class="comfirmbutton">
      <button class="comfirmbutton button" open-type='share'></button>
      <img src="../images/savetips.png" alt="" class="unlogintips">
    </div>


    <div v-if="rankflag" class="showRank">
      <div>
        <!-- <img src="../images/cancel.png" alt="" class="cancelbutton"> -->
        <Rank cname="rankSet" @exit="exit"></Rank>
      </div>
    </div>



    <img :src="tree" alt="" class="tree" mode="aspectFit">

    
  </div>
</template>

<script>
  import Rank from "@/components/rank"
  import { get,post } from '@/until'
  export default{
    data()
    {
      return{
        loginflag : 1,
        usernameflag : 0,
        shareflag : 0,
        rankflag : 0,
        wxnameflag : 0,
        scoreinfo : '',
        tree:'',
        items:[]
      }
    },
    methods:{
      jump_iHD()
      {
        wx.navigateToMiniProgram({
          appId:'wxf55966fb59609a71'
        })
      },
      async wxlogin()
      {
        let self=this;
        let username = wx.getStorageSync('username');
        if(!username)
        {
          wx.navigateTo({
            url:'/pages/login/main'
          })            
        }        
        wx.getUserInfo({
          success: (res)=> {
            console.log(res);
            wx.setStorageSync('wxname',res.userInfo.nickName);
            wx.setStorageSync('wximg',res.userInfo.avatarUrl);
            self.wxnameflag = 0;  
          }
        })                      
      },
      exit() {
        this.rankflag = 0
      },
      async refreshscore()
      {
          let iPlanetDirectoryPro = wx.getStorageSync('iPlanetDirectoryPro');
          let username = wx.getStorageSync("username");
          if(iPlanetDirectoryPro && username)
          {
            wx.showModal({
                title: '提示',
                content: '确定要刷新学分吗',
                success(res){
                  if (res.confirm){
                    console.log('用户点击确定')
                    const url = "https://www.sayetuan.com/KCB/getscore";
                    let data = post(url,{
                      username:username,
                      iPlanetDirectoryPro:iPlanetDirectoryPro,
                      tree:"false"
                    }).then((res)=>{
                      clearInterval(interval);
                      wx.hideToast();
                      return res;
                    },(res)=>{
                      clearInterval(interval);
                      wx.hideToast();
                      return res;
                    });
                    wx.setStorageSync("scoreinfo");
                  }else if(res.cancel){
                    console.log('用户点击取消')
                  }
                }
              })
          }
      },
      logincancel()
      {
        this.wxnameflag = 0;
      },
      sharecancel()
      {
        this.shareflag = 0;
      },
      rankcancel()
      {
        this.rankflag = 0;
      },
      async ifrank()
      {
        let wxname = wx.getStorageSync('wxname');
        let wximg = wx.getStorageSync('wximg');
        let username = wx.getStorageSync('username');
        let share = wx.getStorageSync('share');
        if(!wxname)
        {
          this.wxnameflag = 1;
        }
        else if(!share)
        {
          this.shareflag = 1;
        }
        else
        {
          this.rankflag = 1;
        }
        if(wxname && wximg && username)
        {
          console.log('success');
          const url = "https://www.sayetuan.com/KCB/wx";
          let data = await post(url,{
              username : username,
              wximg : wximg,
              wxname : wxname
          }).then((res)=>{
            return res;
          },(res)=>{
              return res;
          });
        }
      },
      iflogin()
      {
        let username = wx.getStorageSync('username');
        if(username)
        {
          this.loginflag = 0;
        }
      },
      jump_ltxz()
      {
        wx.navigateToMiniProgram({
          appId:'wx1636027207e6fb44',
          path:'pages/index/main',
          extraData: {
            foo: 'bar'
          },
          envVersion: 'release',
          success(res) {
            console.log('success');
          }
        })
      },
      jump_kcb()
      {
        wx.navigateToMiniProgram({
          appId:'wxde2483aafa70e9da',
          path:'pages/index/main',
          extraData: {
            foo: 'bar'
          },
          envVersion: 'release',
          success(res) {
            console.log('success');
          }
        })
      },
      jump_login()
      {
        wx.navigateTo({
          url:'/pages/login/main'
        })
      },
      treecreat()
      {
        if(this.scoreinfo >= 20 && this.scoreinfo <= 26)
        {
          return '../images/1.png'
        }
        else if(this.scoreinfo > 26 && this.scoreinfo <=32)
        {
          return '../images/2.png'
        }
        else if(this.scoreinfo > 32 && this.scoreinfo <= 38)
        {
          return '../images/3.png'
        }
        else if(this.scoreinfo > 38 && this.scoreinfo <= 44)
        {
          return '../images/4.png'
        }
        else if(this.scoreinfo > 44 && this.scoreinfo <= 50)
        {
          return '../images/5.png'
        }
        else if(this.scoreinfo > 50 && this.scoreinfo <= 56)
        {
          return '../images/6.png'
        }
        else if(this.scoreinfo > 56 && this.scoreinfo <= 62)
        {
          return '../images/7.png'
        }
        else if(this.scoreinfo > 62 && this.scoreinfo <= 68)
        {
          return '../images/8.png'
        }
        else if(this.scoreinfo > 68 && this.scoreinfo <= 74)
        {
          return '../images/9.png'
        }
        else if(this.scoreinfo > 74 && this.scoreinfo <= 80)
        {
          return '../images/10.png'
        }
        else if(this.scoreinfo > 80 && this.scoreinfo <= 86)
        {
          return '../images/11.png'
        }
        else if(this.scoreinfo > 86 && this.scoreinfo <= 92)
        {
          return '../images/12.png'
        }
        else if(this.scoreinfo > 92 && this.scoreinfo <= 98)
        {
          return '../images/13.png'
        }
        else if(this.scoreinfo > 98 && this.scoreinfo <= 104)
        {
          return '../images/14.png'
        }
        else if(this.scoreinfo > 104 && this.scoreinfo <= 110)
        {
          return '../images/15.png'
        }
        else if(this.scoreinfo > 110 && this.scoreinfo <= 116)
        {
          return '../images/16.png'
        }
        else if(this.scoreinfo > 116 && this.scoreinfo <= 122)
        {
          return '../images/17.png'
        }
        else if(this.scoreinfo > 122 && this.scoreinfo <= 128)
        {
          return '../images/18.png'
        }
        else if(this.scoreinfo >= 129 && this.scoreinfo <= 134)
        {
          return '../images/19.png'
        }
        else if(this.scoreinfo >= 135)
        {
          return '../images/20.png'
        }
      }
    },
    components: {
      Rank
    },
    // async onLoad() {
    //   let info = await get("http://132.232.202.22/KCB/range")
    //   info = info.data
    //   //let info = {"1":{"score":"93.500000000000000000000000000000","wxname":"彭坤滨","wximg":"https://wx.qlogo.cn/mmopen/vi_32/pdS8qmMU2tjBSVQQiakBiaWyhiajzF82GOUSEiaZWk4LWFuSgCCPvicDD2W8Fhk8xHolOvPTY4GfqNPCp6CoNnoK3YA/132","user":"20164698"}}
    //   let items = []
    //   for(let i = 1; info[i]; i++) {
    //     info[i].rank = i
    //     info[i].score = info[i].score.slice(0, 4)
    //     items.push(info[i])
    //   }
    //   console.log('info', info)
    //   this.items = items
    // },
    async onShow(){
      this.iflogin();
      this.scoreinfo = await wx.getStorageSync('scoreinfo');
      console.log(this.scoreinfo);
      this.tree=this.treecreat();
    },
    onload()
    {
    },
    mounted()
    {

    },
    onShareAppMessage: function(res) {
      let shareimg =[
        '../../images/share1.jpg',
        '../../images/share2.jpg'
      ]
      let randomImg = shareimg [Math.floor(Math.random() * shareimg .length)];
      wx.setStorageSync("share",true)
      this.shareflag = 0
      return {
        title: "快来领取你的黑大专属学分树吧~",
        path: "pages/index/main",
        //imageUrl:'',        
        success: res=> {          
          
        },
        fail: res=> {
          console.log('false')
        }
      };
    }  
  }
</script>

<style scoped>
.iHD{
  position: absolute;
  width: 102rpx;
  height: 102rpx;
  top: 608rpx;
  right: 10rpx;
  z-index:5;  
}
.scoreinfo{
  position: absolute;
  width: 444rpx;
  height: 125rpx;
  top: 53rpx;
  left: 245rpx;
  font-size: 50rpx;
  font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  color: #545358;
  z-index:5;
}
.unlogintips
{
  position: absolute;
  width: 600rpx;
  height: 150rpx;
  left: 0;
  top: 37%;
}
.cancelbutton{
  position: absolute;
  width: 102rpx;
  height: 102rpx;
  right: 0;
  top: 0;
}
.comfirmbutton{
  position: absolute;
  width: 202rpx;
  height: 102rpx;
  right: 35%;
  bottom: 30rpx;
}
.unlogin{
  background: #edd978;
  height: 780rpx;
  width: 630rpx;
  position: absolute;
  top: 200rpx;
  left: 50rpx;
  border-radius: 30rpx;
  border-color: #5b6655;
  border-width:5rpx;
  border-style: solid;
  font-family: 'iconfont';
  font-style: normal;
  z-index:10;
}
.button{
    border: none; 
    background-color: transparent;
    outline: none;
}
.button::after{
    border: none;
}
page{
  width:100%;
  height: 100%;
}
.externallayout{
  position: fixed;
  height: 100%;
  width: 100%;
}
.backgroundimg
{
  width: 100%;
  height: 100%;
}
.kcb
{
  position: absolute;
  width: 102rpx;
  height: 102rpx;
  top: 868rpx;
  right: 10rpx;
  z-index:5;
}
.userscore{
  position: absolute;
  width: 444rpx;
  height: 125rpx;
  top: 38rpx;
  left: 0;
  z-index:5;
}
.rank{
  position: absolute;
  width: 102rpx;
  height: 102rpx;
  top: 38rpx;
  right:125rpx;
  z-index:5;
}
.share{
  position: absolute;
  width: 102rpx;
  height: 102rpx;
  top: 38rpx;
  right: 10rpx;
  z-index:5;
}
.home{
  position: absolute;
  width: 112rpx;
  height: 112rpx;
  bottom: 38rpx;
  right: 45%;
  z-index:5;
}
.ltxz{
  position: absolute;
  width: 102rpx;
  height: 102rpx;
  top: 738rpx;
  right: 10rpx;
  z-index:5;
}
.login{
  position: absolute;
  top: 45%;
  left: 40%;
  border: 0; 
  outline: none;
  z-index:5;
}
.tree{
  position: absolute;
  top: 10%;
  left: 10%;
  border: 0;
  width: 600rpx;
  height:950rpx; 
}
.showRank {
  height: 780rpx;
  width: 630rpx;
  z-index:10;
  position: absolute;
  top: 200rpx;
  left: 50rpx;
}
</style>
