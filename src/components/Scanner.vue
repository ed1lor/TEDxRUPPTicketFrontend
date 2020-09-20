<template>
    <div class= "v-header container">
      <div class="header-content">
        <h1>TedXRUPP, TedSalon</h1>
      </div>
      <div class="fullscreen-video-wrap">
        <video id="video" autoplay></video>
        <canvas id="canvas"></canvas>
      </div>

      <div class="header-overlay"></div>
    </div>
</template>

<script>
//import instascan from "instascan";
export default {
    
    methods: {
        loadScanner() {
            let scanner = new window.Instascan.Scanner({
                video: document.getElementById("video"),
                mirror: false
            });
            scanner.addListener("scan", this.scan);
            window.Instascan.Camera.getCameras()
                .then(function (cameras) {
                    if (cameras.length > 0) {
                        scanner.start(cameras[cameras.length - 1]);
                    } else {
                        console.error("No cameras found.");
                    }
                })
                .catch(function (e) {
                    console.error(e);
                });
        },
                scan(content) {
            if (content == "david") {
                console.log("dope")
            } else {
                console.log("no dope")
            }
        }
    },
    mounted() {
            // Grab elements, create settings, etc.
            var video = document.getElementById('video');

            let instaScanScript = document.createElement("script");
            instaScanScript.src =
                "https://rawcdn.githack.com/tobiasmuehl/instascan/4224451c49a701c04de7d0de5ef356dc1f701a93/bin/instascan.min.js";
            document.body.appendChild(instaScanScript);
            instaScanScript.addEventListener("load", this.loadScanner);

            // Get access to the camera!
            if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
                // Not adding `{ audio: true }` since we only want video now
                navigator.mediaDevices.getUserMedia({
                    video: true
                }).then(function (stream) {
                    //video.src = window.URL.createObjectURL(stream);
                    video.srcObject = stream;
                    video.play();
                });
            }
        }
}
</script>

<style scoped>
    *
{
    box-sizing: border-box;
}
body{
    margin: 0;
    overflow-x: hidden;
}
.v-header {
    height: 100vh;
    display: flex;
    align-items: center;
    flex-direction: column;
    align-content: center;
}
.container{
    max-width: 960px;
    padding-left: 1rem;
    padding-right: 1rem;
    margin: auto;
    text-align: center;
}

.fullscreen-video-wrap {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100vh;
    overflow: hidden;
}

.fullscreen-video-wrap video{
    min-width: 100%;
    min-height: 100%;
}

.header-overlay{
    height: 100vh;
    width: 100vw;
    position: absolute;
    top:0;
    left:0;
    background: crimson;
    z-index: 1;
    opacity:0.2;
}

.header-content {
    z-index: 2;
}

.header-content h1{
    text-align: center;
    font-size: 2rem;
    font-family: "Cookie";
    display: block;
    z-index: 2;
    width: 100%;
}

</style>
