# vue 中引入 videojs

安装 videojs

```js
npm install videojs

// 支持m3u8格式
npm install videojs-contrib-hls
```

vue 中引入.
必须在 mian.js 中引入扩展.不然具体组件中会报错

```js
// main.js

import 'video.js/dist/video-js.css'
import 'videojs-contrib-hls'
```

封装的 oPlayer,在该组件中引入 video.js

```js
// oPlayer.vue

<template>
    <div class="videoPlayer" id="videoPlayer">
        <video id="myvideo" style='width: 100%;height: 100%' class="video-js vjs-default-skin" preload="auto">
        </video>
    </div>
</template>
<script>
    import videojs from 'video.js'
    import {
        onUnmounted
    } from 'vue'
    export default {
        props: {
            videoUrl: {
                type: String,
                default: ''
            }
        },
        setup() {
            onUnmounted(() => {
                videojs.players.myvideo.dispose(); // 销毁videojs
            })
        },
        data() {
            return {
                player: null,
            }
        },
        mounted() {
            var myVideoDiv = document.getElementById("videoPlayer")
            myVideoDiv.innerHTML = "<video id='myvideo'  style='width: 100%;height: 100%' class='video-js vjs-default-skin' preload='auto'></video>"
            this.init()
        },
        methods: {
            init() {
                let options = {
                    controls: true, // 是否显示控制条
                    preload: 'auto',
                    autoplay: false,
                    language: 'zh-CN', // 设置语言
                    muted: false, // 是否静音
                    sources: [{
                        type: 'application/x-mpegURL',
                        src: this.videoUrl
                    }, ],
                    bigPlayButton: true, // 视频中间的播放按钮
                    textTrackDisplay: false,
                    posterImage: false,
                    errorDisplay: false,
                    controlBar: {
                        timeDivider: true,
                        durationDisplay: true, //视频时长时间显示
                        remainingTimeDisplay: false, //剩余时间显示
                        fullscreenToggle: true // 全屏按钮
                    },
                }
                // 播放器初始化
                videojs('myvideo', options)
            }
        },
    }
</script>
<style lang="scss" scoped>
    .videoPlayer {
        height: 390px;
        padding-bottom: 20px;

        .video-js {
            .vjs-big-play-button {
                width: 90px;
            }
        }

    }
</style>
```
