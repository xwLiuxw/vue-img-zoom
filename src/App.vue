<template>
    <div id="app">
        <div class="zoom-box">
            <VueImgZoom
                :url="imgUrl"
                :scale="2"
                :width="400"
                :height="400"
            ></VueImgZoom>
        </div>
    </div>
</template>

<script>
import VueImgZoom from './components/VueImgZoom.vue'
import imgUrl from './assets/logo.png'

export default {
    name: 'app',
    data(){
        return{
            imgUrl
        }
    },
    components: {
        VueImgZoom
    }
}
</script>

<style>
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
.zoom-box {
    border: 1px solid #ccc;
    height: 398px;
    width: 398px;
}
</style>
