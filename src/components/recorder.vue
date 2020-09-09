<template>
    <div class="goeasy-recorder">
        <!--<div :class="[appearanceClass, 'recorder-appearance']" @click="initRecorder">{{text}}</div>-->
        <div class="icon-box recorder-appearance" @click="initRecorder">
            <i class="iconfont fa fa-microphone"></i>
        </div>
        <div class="control-container" v-show="show">
            <span class="record-tips">{{startState ? '点击开始录音' : '点击发送语音'}}</span>
            <span :class="{'record-btn': true, 'active' : !startState}" @click="onStateChange"></span>
            <div class="close-btn" @click="close"></div>
        </div>
        <div class="record-layer" v-show="show"></div>
    </div>
</template>

<script>
    import Recorder from 'recorder-core';
    import 'recorder-core/src/engine/mp3'
    import 'recorder-core/src/engine/mp3-engine'
    export default {
        name: "recorder",
        props : ['text', 'appearanceClass'],
        data () {
            return {
                audio : null,
                show : false,
                startState : true
            }
        },
        methods : {
            initRecorder () {
                this.show = true;
                this.audio = Recorder({
                    type : 'mp3',
                    sampleRate:16000,
                    bitRate:16,
                    onProcess : function () {

                    }
                });
                this.audio.open(() => {
                    //授权成功
                    console.log("授权成功")
                }, () => {
                    //授权失败
                    alert("授权失败")
                    this.close()
                })
            },
            onStateChange() {
                if(this.startState) {
                    this.start();
                    this.startState = false
                }else{
                    this.stop();
                    this.startState = true;
                }
            },
            start () {
                this.startState = false;
                this.audio.start()
            },
            stop () {
                this.startState = true;
                this.show = false;
                this.audio.stop((blob, duration) => {
                    this.audio.close();
                    this.audio = null;
                    let file = new File([blob], 'audio.mp3', {type: blob.type, lastModified: Date.now(), duration});
                    this.$emit('onComplete', {
                        file,
                        duration
                    });
                }, (msg) => {
                    console.log(msg);
                    this.audio.close();
                    this.audio = null;
                })
            },
            close () {
                this.startState = true;
                this.show = false;
                this.audio.close();
                this.audio = null;
            }
        }
    }
</script>

<style scoped>
    .icon-box {
        color: #aaa;
        font-size: 20px;
    }
    .recorder-appearance{
        moz-user-select: -moz-none;
        -moz-user-select: none;
        -o-user-select:none;
        -khtml-user-select:none;
        -webkit-user-select:none;
        -ms-user-select:none;
        user-select:none;
    }
    .control-container{
        display: flex;
        position: fixed;
        bottom: 3%;
        left: 50%;
        transform: translate(-50%, 0);
        justify-content: space-around;
        z-index: 999;
        flex-direction: column;
    }

    .record-btn{
        width: 64px;
        height: 64px;
        border:4px solid #ffffff;
        text-align: center;
        position: relative;
        z-index: 999;
        margin: 20px 0;
        -webkit-tap-highlight-color: transparent;
        moz-user-select: -moz-none;
        -moz-user-select: none;
        -o-user-select:none;
        -khtml-user-select:none;
        -webkit-user-select:none;
        -ms-user-select:none;
        user-select:none;
        background: url("../../static/image/record.png") no-repeat center;
        background-size: 27px 33px;
        border-radius: 50%;
    }
    .active{
        background: #ffffff url("../../static/image/record-active.png") no-repeat center;
        background-size: 27px 33px;
        border-radius: 50%;
    }
    .record-layer{
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: gray;
        font-size: 16px;
        color: #ffffff;
        opacity: 0.5;
        z-index: 998;
    }
    .close-btn{
        width: 64px;
        height: 64px;
        background: url("../../static/image/close.png") no-repeat center;
        background-size: 22px 22px;
        -webkit-tap-highlight-color:rgba(0,0,0,0);
    }
    .record-tips{
        text-align: center;
        color: #ffffff;
    }
</style>
