<template>
    <div id="content">
        <div id="p5Canvas"></div>
    </div>
</template>
  
<script>
import p5 from "p5";
import ml5 from "ml5";

export default {
    name: "App",
    data() {
        return {
            modelUrl: "https://teachablemachine.withgoogle.com/models/HKOaqhg7w/",
            label: 'Loading...'
        }
    },
    onLoad() {

    },
    mounted() {
        let d = new p5(this.defaultSketch, "p5Canvas");
    },
    methods: {
        defaultSketch(s) {
            let that = this
            let video;
            let classifier;

            s.setup = function () {
                s.createCanvas(750, 600);
                video = s.createCapture(s.VIDEO);
                video.size(750, 600);
                video.hide();
                classifier = ml5.imageClassifier(that.modelUrl + "model.json", video, modelReady);
            };

            s.draw = function () {
                s.background(0);
                s.image(video, 0, 0);
                s.fill(255);
                s.textAlign(s.CENTER);
                s.textFont('Georgia');
                s.textSize(32);
                s.text(that.label, 400, 580);
            };

            function modelReady() {
                classifier.predict(gotResults);
            };

            function gotResults(error, results) {
                if (!error) {
                    that.label = results[0].label;
                    uni.$emit('scanFinished', {label: results[0].label});
                    setTimeout(function () {
                        uni.navigateBack();
                    }, 2000);
                }
            }
        },

    }
};
</script>

<style>
#content {
    width: 750px;
    height: 1624px;
    padding: 0 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
}
</style>