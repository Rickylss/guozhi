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
    width: 376px;
    height: 812px;
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
    width: 48px;
    height: 46px;
    margin-top: 48px;
    margin-left: 8px;
    border-radius: 48px;
    background-color: transparent;
    background-image: url("/static/images/Coin.svg");
    background-repeat: no-repeat;
    background-size: cover;
    z-index: 8000;
}

.stage .box {
    position: absolute;
    left: 17px;
    top: 56px;
    width: 110px;
    height: 32px;
    background-color: #ffffff;
    border-radius: 16px;
    z-index: 7000;
}

.stage .box text {
    position: relative;
    left: 40px;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 600;
    font-size: 10px;
    text-align: center;
    line-height: 32px;
    color: #227391;
}

.status {
    margin-top: 56px;
    margin-right: 12px;
    width: 210px;
    height: 34px;
    border-radius: 28px;
    background-color: #ffffff;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

.status .attributes {
    margin-right: 12px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

.status .attributes view {
    margin-right: 6px;
    margin-top: 2px;
    width: 32px;
    height: 32px;
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
    font-size: 10px;
    font-family: 'PingFang SC';
    text-align: center;
    color: #303030;
}

.status_v button {
    margin-right: 12px;
    margin-top: 8px;
    background-color: transparent;
    background-size: cover;
    border-radius: 38px;
    width: 38px;
    height: 38px;
}

.screen_shot {
    background-image: url("/static/images/screen-shot.svg");
}

.hint {
    background-image: url("/static/images/hint.svg");
}

.message {
    margin-right: 12px;
    margin-top: 8px;
    background: rgba(103, 103, 103, 0.5);
    width: 248px;
    height: 42px;
    border-radius: 8px;
    align-self: flex-end;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    color: #ffffff;
}

.start_button {
    width: 126px;
    height: 46px;
    background-image: url("/static/images/Play%20Button.svg");
    background-repeat: no-repeat;
    background-size: cover;
    background-color: transparent;
    border-radius: 50px;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 500;
    font-size: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ffffff;
    margin-bottom: 112px;
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
    width: 334px;
    height: 600px;
    top: 140px;
    left: 20px;
    z-index: 9999;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.modal_c .modal_dlg {
    width: 334px;
    height: 444px;
    background-image: url("/static/images/Popup.svg");
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}

.modal_dlg .text1 {
    margin-top: 82px;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    text-align: center;
    color: #333333;
}

.modal_c .text2 {
    margin-top: 4px;
    width: 250px;
    height: 34px;
    font-family: 'PingFang SC';
    font-style: normal;
    font-weight: 400;
    font-size: 12px;
    color: #666666;
    opacity: 0.77;
}

.modal_dlg view {
    width: 232px;
    height: 186px;
    margin-top: 18px;
    background-image: url("/static/images/sample.svg");
    background-repeat: no-repeat;
    background-size: cover;
}

.modal_dlg button {
    width: 126px;
    height: 46px;
    margin-top: 18px;
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
    font-size: 16px;
    color: #ffffff;
    border-radius: 100px;
}

.modal_c .modal_cancel {
    width: 52px;
    height: 52px;
    border-radius: 52px;
    background-image: url("/static/images/close.svg");
    background-repeat: no-repeat;
    background-size: cover;
    background-color: transparent;
    margin-top: 10px;
}
</style>