<template>
    <section>
        <article class="item kcb" @click="toKcb">
            <div>
                <img src="../../images/kcb.png" alt="">
            </div>
            <div class="text">
                课程表<br>
                黑大专属
            </div>
        </article>
        <article class="item hdtree">
            <div>

            </div>
            <div class="text">
                黑大树<br>
                (开发中)
            </div>
        </article>
        <article class="item logo">
            <div>
                <img src="../../images/nyg.png" alt="">
            </div>
        </article>
        <article class="item link" @click="link">
            <div class="textInfo">
                <img src="../../images/aboutUs.png" alt="">
            </div>
        </article>
        <article class="item return">
            <div>
                <img src="../../images/suggestion.png" alt="">
            </div>
        </article>
    </section>
</template>

<script>
export default {
    methods: {
        link() {
            wx.navigateTo({ url:"../linkUs/main" })
        },
        toKcb() {
            wx.navigateToMiniProgram({
                appId: 'wx9dfd0ee8c2417c1a',
            })
        }
    }
}
</script>

<style lang = "scss" scoped>
    .item {
        position: absolute;
        height: 198rpx;
        width: 214rpx;
        div {
            width: 100%;
            height: 100%;
            img {
                width: 100%;
                height: 100%;
            }
        }
        .text {
            text-align: center;
            font-family: PingFangSC-Regular;
            color: #5E5C5C;
            font-size: 32rpx;

        }
    }
    .kcb {
        left: 80rpx;
        top: 180rpx;
    }
    .hdtree {
       left: 472rpx;
       top: 180rpx;
    }
    .logo {
        left: 166rpx;
        top:470rpx;
        width: 360rpx;
        height: 360rpx;
    }
    .link {
        left: 76rpx;
        top: 920rpx;
    }
    .return {
        left: 438rpx;
        top: 920rpx;
    }
    .textInfo {
        text-align: center;

    }
</style>


