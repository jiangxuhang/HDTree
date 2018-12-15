<template>
    <section class="container" :class="cname">
        <h2>排行</h2>
        <scroll-view scroll-y class="scroll" v-if="model == 1">
            <article v-for="(item, index) in info" :key="index">
                <div class="left img">
                    <img :src="item.wximg" alt="">
                </div>
                <div class="center">
                    <div class="name">{{item.wxname}}</div>
                </div>
                <div class="right score">学分:{{item.score}}</div>
            </article>
            <div class="linkWorld" @click="switchmodel">查看世界排行</div>
        </scroll-view>
        <button class="btn_y" @click="primary" v-if="model == 1">确定</button>
        <scroll-view scroll-y class="scroll" v-if="model == 2">
            <article v-for="(item, index) in info" :key="index">
                <div class="left img">
                    <img :src="item.wximg" alt="">
                </div>
                <div class="center">
                    <div class="name">{{item.wxname}}</div>
                    <div class="rank" v-if="model == 2">排行:{{item.rank}}</div>
                </div>
                <div class="right score">学分:{{item.score}}</div>
            </article>
        </scroll-view>
        <footer class="fixFoot" v-if="model == 2">
            <button @click="primary">确认</button>
            <button @click="switchmodel">我的排名</button>
        </footer>
    </section>
</template>

<script>
import {get, post} from "@/until"
export default {
    data() {
        return {
            info:[],
            show: true,
            model: 1
        }
    },
    props: {
        cname: "String",
    },
    async created() {
        let info = await get("https://www.sayetuan.com/KCB/range")
        info = info.data
        //let info = {"1":{"score":"93.500000000000000000000000000000","wxname":"彭坤滨","wximg":"https://wx.qlogo.cn/mmopen/vi_32/pdS8qmMU2tjBSVQQiakBiaWyhiajzF82GOUSEiaZWk4LWFuSgCCPvicDD2W8Fhk8xHolOvPTY4GfqNPCp6CoNnoK3YA/132","user":"20164698"}}
        let items = []
        for(let i = 1; info[i]; i++) {
            info[i].rank = i
            info[i].score = info[i].score.slice(0, 5)
            items.push(info[i])
        }
        console.log('info', info)
        this.items = items

        let rank = {}
        rank.wxname =  wx.getStorageSync('wxname')
        rank.wximg = wx.getStorageSync('wximg')
        rank.score = wx.getStorageSync("scoreinfo");
        this.info.push(rank)
        console.log('info',this.info)
    },
    methods: {
        primary() {
            this.$emit("exit")
        },
        switchmodel() {
            if(this.model == 1) {
                this.model = 2
                this.info = this.items
                console.log("item", this.items)
            } else {
                this.model = 1
                let info = {}
                info.wxname =  wx.getStorageSync('wxname')
                info.wximg = wx.getStorageSync('wximg')
                info.score = wx.getStorageSync("scoreinfo");
                this.info = []
                this.info[0] = info
                console.log('info',this.info)
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.container {
    width: 100%;
    height: 100%;
    border: 1px solid #5b6655;;
    margin:0 auto;
    box-sizing: border-box;
    padding: 30rpx;
    border-radius: 30rpx;
    background: #edd978;
    position: relative;
    z-index: 100;
    .linkWorld {
        text-align: center;
        margin-top: 200rpx;
    }
    .name {
        width: 190rpx;
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
    }
    .btn_y {
        font-size: .33rem;
        width: 160rpx;
        height: 74rpx;
        line-height: 74rpx;
        border-radius: 40rpx;
        z-index: 90;
    }
    .fixFoot {
        display: flex;
        button {
            width: 200rpx;
            border-radius: 50rpx;
            height: 74rpx;
            line-height: 74rpx;
            &:nth-child(1) {
                font-size: .33rem;
                margin-right: 30rpx;
            }
            &:nth-child(2) {
                font-size: .33rem;
            }
        }
    }
    &>h2 {
        text-align: center;
        //border-bottom: 2px solid black;
    }
    article {
        width: 100%;
        display: flex;
        justify-content: space-between;
        border-bottom: 1rpx solid black;
        padding: 20rpx 0;
        .img {
            width: 100rpx;
            height: 100rpx;
            img {
                width:100%;
                height:100%;
                border-radius: 50%;
            }
        }
    }
}
</style>

<style lang="scss">
::-webkit-scrollbar {
    width: 0;
    height: 0;
    color: transparent;
}
.scroll {
        height: 60vh;
}
</style>

