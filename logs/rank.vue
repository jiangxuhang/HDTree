<template>
    <section class="container" v-if="show">
        <h2>排行</h2>
        <scroll-view scroll-y style="height: 1000rpx" class="scroll" v-if="model == 1">
            <article v-for="(item, index) in info" :key="index">
                <div class="left img">
                    <img :src="item.avatarUrl" alt="">
                </div>
                <div class="center">
                    <div class="name">{{item.nickName}}</div>
                    <div class="rank">排行:{{item.rank}}</div>
                </div>
                <div class="right score">学分:{{item.score}}</div>
            </article>
            <div class="linkWorld" @click="switchmodel">查看世界排行</div>
            <button class="btn_y" @click="primary">确定</button>
        </scroll-view>
        <scroll-view scroll-y style="height: 700rpx" class="scroll" v-if="model == 2">
            <article v-for="(item, index) in info" :key="index">
                <div class="left img">
                    <img :src="item.avatarUrl" alt="">
                </div>
                <div class="center">
                    <div class="name">{{item.nickName}}</div>
                    <div class="rank">排行:{{item.rank}}</div>
                </div>
                <div class="right score">学分:{{item.score}}</div>
            </article>
            <footer class="fixFoot">
                <button @click="primary">确认</button>
                <button @click="switchmodel">我的排名</button>
            </footer>
        </scroll-view>
    </section>
</template>

<script>
export default {
    data() {
        return {
            info:[{
                nickName: "c-moon",
                score: 20,
                avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                rank: 1
            }],
            show: true,
            model: 1
        }
    },
    methods: {
        primary() {
            this.show = false;
        },
        switchmodel() {
            if(this.model == 1) {
                this.model = 2
                this.info = [{
                    nickName: "c-moon",
                    score: 20,
                    avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                    rank: 1
                },{
                    nickName: "c-moon",
                    score: 20,
                    avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                    rank: 1
                },{
                    nickName: "c-moon",
                    score: 20,
                    avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                    rank: 1
                },{
                    nickName: "c-moon",
                    score: 20,
                    avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                    rank: 1
                },{
                    nickName: "c-moon",
                    score: 20,
                    avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                    rank: 1
                },{
                    nickName: "c-moon",
                    score: 20,
                    avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                    rank: 1
                },{
                    nickName: "c-moon",
                    score: 20,
                    avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                    rank: 1
                },{
                    nickName: "c-moon",
                    score: 20,
                    avatarUrl: "https://wx.qlogo.cn/mmopen/vi_32/g4KkEzaFibOZIyjM4tPCD7ovdMHJaos1BUbQwNF0WjwYg9d31xLsvEp4CACGHryYJMksia0CVrRfXw6yZS9BGrAQ/132",
                    rank: 1
                }]
            } else {
                this.model = 1
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.container {
    width: 80%;
    height: 1000rpx;
    border: 1px solid black;
    margin:0 auto;
    margin-top: 150rpx;
    box-sizing: border-box;
    padding: 30rpx;
    border-radius: 30rpx;
    background-color: rgb(228, 183, 99);
    position: relative;
    overflow: hidden;
    .scroll {
    }
    .linkWorld {
        text-align: center;
        margin-top: 300rpx;
    }
    .btn_y {
        width: 160rpx;
        height: 74rpx;
        line-height: 74rpx;
        border-radius: 40rpx;
        margin-top: 200rpx;
    }
    .fixFoot {
        button {
            width: 200rpx;
            border-radius: 50rpx;
            height: 74rpx;
            line-height: 74rpx;
            &:nth-child(1) {
                position: fixed;
                bottom: 100rpx;
                left: 100rpx;
            }
            &:nth-child(2) {
                position: fixed;
                bottom: 100rpx;
                right: 100rpx;
            }
        }
    }
    &>h2 {
        text-align: center;
        border-bottom: 2px solid black;
    }
    article {
        width: 100%;
        display: flex;
        justify-content: space-between;
        border-bottom: 2px solid black;
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
</style>

