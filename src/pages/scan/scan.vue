<template>
    <div id="app">
        <div id="p5Canvas"></div>
    </div>
</template>
  
<script>
import p5 from "p5";
import ml5 from "ml5";

const sketch = (s) => {

    let modelUrl = "https://teachablemachine.withgoogle.com/models/hrv3Adexv/";
    let label = "Loading...";
    let video;
    let classifier;

    s.setup = function () {
        s.createCanvas(800, 600);
        video = s.createCapture(s.VIDEO);
        video.size(800, 600);
        video.hide();
        classifier = ml5.imageClassifier(modelUrl + "model.json", video, modelReady);
    };

    s.draw = function () {
        s.background(0);
        s.image(video, 0, 0);
        s.fill(255);
        s.textAlign(s.CENTER);
        s.textFont('Georgia');
        s.textSize(32);
        s.text(label, 400, 580);
    };

    function modelReady() {
        classifier.predict(gotResults);
    };

    function gotResults(error, results) {
        if (!error) {
            label = results[0].label;
            classifier.predict(gotResults);
        }
    }

};

let d;
export default {
    name: "App",

    beforeMount() {
        d = new p5(sketch, "p5Canvas");
    },

    mounted() {
        setTimeout(() => {
            d.background("red");
        }, 1000);
    },
};
</script>