<template>
    <div class="goeasy-audio-player" @click="playAudio">
        <div class="audio-facade" :style="{width:Math.ceil(duration/1000)*5 + 80 + 'px'}">
            <span class="audio-facade-bg"> </span>
            <span>{{Math.ceil(duration/1000)}}"</span>
        </div>
        <audio ref="audioPlayer"></audio>
    </div>
</template>

<script>
    export default {
        name: "audioPlayer",
        props : ['src', 'duration'],
        data () {
            return {
            }
        },
        methods : {
            playAudio () {
                this.$refs.audioPlayer.src =this.src;
                this.$refs.audioPlayer.currentTime = 0;
                this.$refs.audioPlayer.play();
            }
        }
    }
</script>

<style scoped>
    .goeasy-audio-player{

    }
    .audio-facade{
        min-width: 20px;
        padding: 3px 10px;
        height:26px;
        line-height: 20px;
        font-size: 12px;
        border-radius: 15px;
        color: #ffffff;
        -webkit-tap-highlight-color:rgba(0,0,0,0);
        display: flex;
    }
    .audio-facade-bg{
        background: url("../../static/image/voice.png") no-repeat center;
        background-size: 18px;
        width: 20px;
    }
</style>
