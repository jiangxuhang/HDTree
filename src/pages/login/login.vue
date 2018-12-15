<template>
    <div class="externallayout">
        <img src="../images/login.png" class="backgroundimg">
        <input type="text" class="username" v-model="username">
        <input type="password" name="" id="" class="userpassword" v-model="password">
        <span class="testnumber">体验帐号:1234</span>
        <span class="testpassword"> 密码:1234</span>
        <button @click="prelogin" class="button"></button>
    </div>
</template>
<script>
import { get,post } from '@/until'
export default {
    data(){
        return{
            username:'',
            password:''
        }
    },
    methods:{
        async prelogin()
        {
            console.log(this.username,this.password)
            if("" == this.username)
            {
                wx.showToast({
                    title:"学号不能为空噢",
                    icon:"none",
                });
            }
            else if("" == this.password)
            {
                wx.showToast({
                    title:"密码不能是空吧",
                    icon:"none",
                });
            }
            else if(this.username === '1234'&&this.password ==='1234')
            {
                await wx.setStorageSync('scoreinfo','74');
                await wx.setStorageSync("username",this.username);
                wx.redirectTo({url:"../index/main"});
            }
            else
            {
                this.login();
            }
        },
        async getcredit()
        {
            let iPlanetDirectoryPro = wx.getStorageSync('iPlanetDirectoryPro');
            let username = wx.getStorageSync("username");
            const url = "https://www.sayetuan.com/KCB/getscore";
            let data = await post(url,{
                username:username,
                iPlanetDirectoryPro:iPlanetDirectoryPro,
                tree:"true"
            }).then((res)=>{
                console.log(res.data)
                wx.setStorageSync("scoreinfo",res.data);
                return res;
            },(res)=>{
                return res;
            });
        },
        async login()
        {
            wx.showToast({
                title:"正在登陆",
                icon:"loading",
                duration: 5000,
            });
            let interval = setInterval(()=>{
            wx.showToast({
                title:"正在登陆",
                icon:"loading",
                duration: 5000,
            });
            },5000);
            await wx.setStorageSync("interval",interval);
            const url = "https://www.sayetuan.com/KCB/Login";
            let data = await post(url,{
                username:this.username,
                password:this.password,
            }).then((res)=>{
                clearInterval(interval);
                wx.hideToast();
                return res;
            },(res)=>{
                clearInterval(interval);
                wx.hideToast();
                return res;
            });
            console.log(data)
            if(!data.data.iPlanetDirectoryPro) {
                return wx.showToast({
                title:'登陆失败',
                icon:'none',
            });
            }
            clearInterval(interval);
            wx.hideToast();
            await wx.setStorageSync("username",this.username);
            await wx.setStorageSync("iPlanetDirectoryPro",data.data.iPlanetDirectoryPro);
            await this.getcredit();
            wx.redirectTo({url:"../index/main"});
        },        
    },
}
</script>
<style>
.testnumber{
    position: absolute;
    top:78%;
    left:23%;
    font-size: 28rpx;
    width:220rpx;
    height:100rpx;
    font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    opacity: 0.8;
}
.testpassword{
    position: absolute;
    top:78%;
    left:53%;
    font-size: 28rpx;
    width:220rpx;
    height:100rpx;
    font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    opacity: 0.8;
}
.username{    
	background:none;  
	outline:none;  
	border:0px;
    position: absolute;
    top:54%;
    left:33%;
    font-size: 48rpx
}
.userpassword{
	background:none;  
	outline:none;  
	border:0px;
    position: absolute;
    top:69%;
    left:33%;
    font-size: 48rpx
}
.button{
    position: absolute;
    top:83.5%;
    left:32.5%;
    border: none; 
    background-color: transparent;
    outline: none;
    height: 115rpx;
    width: 275rpx;
}
.button::after{
    border: none;
} 
</style>
