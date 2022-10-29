<template>
    <view class="content">
        <view class="status_v">
            <view class="line">
                <view class="stage">
                    <view class="coin"></view>
                    <view class="box"><text>{{ stage }}</text></view>
                </view>
                <view class="status">
                    <view class="attributes">
                        <view class="sunlight"></view>
                        <text>{{ sunlight }}</text>
                    </view>
                    <view class="attributes">
                        <view class="raindrop"></view>
                        <text>{{ raindrop }}</text>
                    </view>
                    <view class="attributes">
                        <view class="temp"></view>
                        <text>{{ temp }}</text>
                    </view>
                </view>
            </view>
            <button class="screen_shot"></button>
            <button class="hint" @click="showHint()"></button>
            <view class='message' v-if="show">{{ message }}</view>
        </view>

        <button class="start_button" @click="startCultivate()">开始培养</button>

        <view class="modal_c" v-if="showModal">
            <view class="modal_dlg">
                <text class="text1">给植物拟定阳光、雨水和温度吧</text>
                <text class="text2">从左向右涂是从低到高呦，在合适的阳光、雨水和温度处停笔吧（我们拿雨水做例子）</text>
                <view></view>
                <button @click="goTo('/pages/scan/scan')">去涂卡({{ time }}s)</button>
            </view>
            <button class="modal_cancel" @click="hideModal()"></button>
        </view>
        <view class="mask" v-if="showModal" catchtouchmove="preventTouchMove"></view>
    </view>

</template>

<script>
export default {
    data() {
        return {
            show: false,
            message: "小贴士：苹果幼苗期更喜欢雨水呦~",
            showModal: false,
            time: 10,
            stage: "幼苗生长期",
            sunlight: "-",
            raindrop: "-",
            temp: "-"
        }
    },
    onLoad() {
        uni.$on('scanFinished', this.update)
    },
    onUnload() {
        uni.$off('scanFinished', this.update)
    },
    methods: {
        showHint() {
            var that = this;
            that.show = true;
            setTimeout(function () {
                that.show = false;
            }, 3000);
        },

        preventTouchMove() {

        },

        startCultivate() {
            var that = this;
            that.showModal = true;
        },

        hideModal() {
            var that = this;
            that.showModal = false;
        },

        goTo(url) {
            this.hideModal();
            uni.navigateTo({
                url: url,
            });
        },

        update(data) {
            console.log(data.label);
            if (data.label == "凉爽" ||
                data.label == "寒冷" ||
                data.label == "适宜" ||
                data.label == "炎热") {
                this.temp = data.label;
            }
            if (data.label == "少量" ||
                data.label == "中等" ||
                data.label == "较多" ||
                data.label == "特多") {
                this.raindrop = data.label;
            }
            if (data.label == "短日照" ||
                data.label == "中日照" ||
                data.label == "较长日照" ||
                data.label == "长日照") {
                this.sunlight = data.label;
            }
        }
    }
}
</script>

<style>
.content {
    width: 750px;
    height: 1624px;
    background-image: url('/static/images/bg.svg');
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

}

.status_v {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.status_v .line {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin: auto 0;
    background-color: transparent;
}

.stage {
    width: auto;
    display: flex;
    flex-direction: row;
    background-color: transparent;
    align-items: center;
}

.stage .coin {
    width: 96.4px;
    height: 94px;
    margin-top: 97px;
    margin-left: 17px;
    border-radius: 96px;
    background-color: transparent;
    background-image: url("/static/images/Coin.svg");
    background-repeat: no-repeat;
    background-size: cover;
    z-index: 8000;
}

.stage .box {
    position: absolute;
    left: 34px;
    top: 110px;
    width: 220px;
    height: 64px;
    background-color: #ffffff;
    border-radius: 31.5px;
    z-index: 7000;
}

.stage .box text {
    position: relative;
    left: 80px;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 600;
    font-size: 20px;
    text-align: center;
    line-height: 64px;
    color: #227391;
}

.status {
    margin-top: 110px;
    margin-right: 24px;
    width: 420px;
    height: 67px;
    border-radius: 47px;
    background-color: #ffffff;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

.status .attributes {
    margin-right: 24px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

.status .attributes view {
    margin-right: 10px;
    margin-top: 4px;
    width: 62.72px;
    height: 63px;
    background-repeat: no-repeat;
    background-size: cover;
    position: relative;
}

.status .attributes .sunlight {
    background-image: url("/static/images/sunlight.svg");
}

.status .attributes .raindrop {
    background-image: url("/static/images/raindrop.svg");
}

.status .attributes .temp {
    background-image: url("/static/images/temp.svg");
}

.status .attributes text {
    font-weight: 500;
    font-size: 20px;
    font-family: 'PingFang SC';
    text-align: center;
    color: #303030;
}

.status_v button {
    margin-right: 24px;
    margin-top: 16px;
    background-color: transparent;
    border-radius: 76px;
    width: 76px;
    height: 76px;
}

.screen_shot {
    background-image: url("/static/images/screen-shot.svg");
}

.hint {
    background-image: url("/static/images/hint.svg");
}

.message {
    margin-right: 24px;
    margin-top: 16px;
    background: rgba(103, 103, 103, 0.5);
    width: 498px;
    height: 84px;
    border-radius: 16px;
    align-self: flex-end;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 400;
    font-size: 28px;
    color: #ffffff;
}

.start_button {
    width: 252px;
    height: 92px;
    background-image: url("/static/images/Play%20Button.svg");
    background-repeat: no-repeat;
    background-size: cover;
    background-color: transparent;
    border-radius: 100px;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 500;
    font-size: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ffffff;
    margin-bottom: 226px;
}

.mask {
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    background: #000000;
    z-index: 9000;
    opacity: 0.6;
}

.modal_c {
    position: fixed;
    width: 668px;
    height: 1200px;
    top: 280px;
    left: 41px;
    z-index: 9999;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.modal_c .modal_dlg {
    width: 668px;
    height: 888px;
    background-image: url("/static/images/Popup.svg");
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}

.modal_dlg .text1 {
    margin-top: 164px;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 400;
    font-size: 28px;
    text-align: center;
    color: #333333;
}

.modal_c .text2 {
    margin-top: 7px;
    width: 503px;
    height: 68px;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 400;
    font-size: 24px;
    color: #666666;
    opacity: 0.77;
}

.modal_dlg view {
    width: 464px;
    height: 371px;
    margin-top: 35px;
    background-image: url("/static/images/sample.svg");
    background-repeat: no-repeat;
    background-size: cover;
}

.modal_dlg button {
    width: 252px;
    height: 92px;
    margin-top: 36px;
    background-image: url("/static/images/Play%20Button.svg");
    background-color: transparent;
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 500;
    font-size: 30px;
    color: #ffffff;
    border-radius: 100px;
}

.modal_c .modal_cancel {
    width: 106px;
    height: 106px;
    border-radius: 106px;
    background-image: url("/static/images/close.svg");
    background-repeat: no-repeat;
    background-size: cover;
    background-color: transparent;
    margin-top: 21px;
}
</style>