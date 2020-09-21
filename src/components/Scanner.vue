<template>

    <div class="v-header container">
        <div id="splash">
            <img src="../assets/tedxrupp-logo.svg" />
        </div>
        <div class="header-content">
            <h1>TedXRUPP, TedSalon</h1>
        </div>
        <div class="fullscreen-video-wrap">
            <video id="video" autoplay></video>
         
        </div>

        <!-- <div class="header-overlay"></div> -->
    </div>
</template>

<script>
//import instascan from "instascan";
import Vue from "vue";
import VueSimpleAlert from "vue-simple-alert";
 //ES6 Modules or TypeScript
import Swal from 'sweetalert2'

// CommonJS
Vue.use(VueSimpleAlert);
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
                Swal.fire({
                    icon: 'success',
                    title: 'Hooray!',
                    text: 'You have successfully redeemed your ticket.',
                    confirmButtonColor: '#e62b1e'
                })
            } else {
                console.log("no dope")
                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: 'Something went wrong!',
                    confirmButtonColor: '#e62b1e'
                })
            }
        }
    },
    mounted() {

            const splashScreen = document.getElementById("splash");
            let visited = sessionStorage.getItem("visited");
            if (!visited) {
                let loadingInterval = setInterval(() => {
                    splashScreen.classList.add("loaded");
                    clearInterval(loadingInterval);
                    sessionStorage.setItem("visited", true);
                }, 1500);
            } else {
                splashScreen.classList.add("loaded");
            }
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
body{
    padding: 0;
    margin: 0;
    overflow: hidden;
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
    position: fixed;
    height: 100vh;
    width: 100%;
    object-fit: cover;
    margin: 0;
    padding: 0;
    top: 0;
    left: 0;
}

.fullscreen-video-wrap video{
    min-width: 100%;
    min-height: 100vh;
}

/* .header-overlay{
    height: 100vh;
    width: 100vw;
    position: absolute;
    top:0;
    left:0;
    background: crimson;
    z-index: 1;
    opacity:0.85;
} */

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
    overflow-y: hidden;
}

#splash {
    position: absolute;
    width: 100vw;
    height: 100vh;
    background-color: #202020;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10000;
    overflow: hidden;
    top: 0;
    left: 0;
}

.loaded {
    display: none !important;
}

img {
    width: 170px;
    display: flex;
}

</style>
