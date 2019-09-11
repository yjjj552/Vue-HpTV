<template>
  <div class="item" >

    <div class="player" >
      <video-player   class="vjs-custom-skin"   :options="playerOptions"  >
      </video-player>
    </div>
    <div class="stage">
      <vue-baberrage
        :isShow= "barrageIsShow"
        :barrageList = "barrageList"
        :loop = "barrageLoop"
        :maxWordCount = "60"
      >
      </vue-baberrage>
    </div>


    <div class="demo-control">
      <div>
        <div>
        <input type="text" v-model="name"></input>
        <button type="button" @click="initWebSocket">连接</button>
        </div>
        <input type="text" style="float:left"  v-model="msg" />
        <button type="button" style="float:left" @click="addToList">Add</button>
      </div>
    </div>
  </div>
</template>

<script>
    import { getList } from '@/api/table'
    import {rtmp} from "@/api/user";
    import barrage from "./barrage/barrage";
    import { MESSAGE_TYPE } from 'vue-baberrage'
    export default {
        filters: {
            statusFilter(status) {
                const statusMap = {
                    published: 'success',
                    draft: 'gray',
                    deleted: 'danger'
                }
                return statusMap[status]
            }
        },
        components:{
          barrage
        },
        data() {
            return {
                timer:1,
                list: null,
                listLoading: true,
                playerOptions: {
                    height: '800',
                    sources: [{
                        type: 'rtmp/flv',
                        src: 'rtmp://47.96.237.94:1935/live/mylive'
                    }],
                    techOrder: ['flash'],
                    muted: false, // 默认静音
                    preload: 'auto', // 视频预加载
                    autoplay: true,
                    notSupportedMessage: '今天没有公开课!',
                    controlBar: {
                        timeDivider: true,
                        durationDisplay: true,
                        remainingTimeDisplay: false,
                        fullscreenToggle: true  //全屏按钮
                    },
                    poster: "https://surmon-china.github.io/vue-quill-editor/static/images/surmon-9.jpg",
                    loop: false
                },
                msg: '',
                barrageIsShow: true,
                currentId : 0,
                barrageLoop: false,
                barrageList: [],
                position: 'abc',
                websocket:'',
                name:''
            }
        },
        created() {
            // this.fetchData()

        },
        destroyed() {
            this.websocketClose();
        },
        methods: {
            fetchData() {
                rtmp().then( (re) =>{
                    this.playerOptions.sources[0].src = "rtmp://47.96.237.94:1935/live/mylive"
                }).catch( (re) =>{
                    console.log(re)
                })
            },
            addToList () {
            let messageData = {
                chatUser: this.name,
                chatContent: this.msg
            };
                this.chatRoomWebsocket.send(JSON.stringify(messageData));
                //弹幕
                this.barrageList.push({
                    id: ++this.currentId,
                    msg: this.msg,
                    time: 10,
                    type: 0,
                });
            },
            // 初始化 websocket 连接
            initWebSocket() {
                console.log(this.name)
                if (typeof WebSocket != "undefined") {
                    this.supported = "支持 websocket";
                } else {
                    this.supported = "不支持 websocket";
                }
                //ws地址
                const wsuri = "ws://localhost:8080/websocket/" + this.name;
                this.chatRoomWebsocket = new WebSocket(wsuri);
                this.chatRoomWebsocket.onerror = this.websocketOnError;
                this.chatRoomWebsocket.onmessage = this.websocketOnMessage;
                this.chatRoomWebsocket.onclose = this.websocketOnClose;
            },
            //关闭
            websocketClose() {
                this.chatRoomWebsocket.close();
            },

            //连接关闭的回调方法
            websocketOnClose(e) {
                console.log("WebSocket 连接关闭", e);
            },
            //接收到消息的回调方法
            websocketOnMessage(event) {

                let data = JSON.parse(event.data);
                console.log(data)
                if (data.chatUser !== this.name){
                    this.barrageList.push({
                        id: ++this.currentId,
                        msg: data.chatContent,
                        // barrageStyle: "normal",
                        time: 10,
                        type: 0,
                        // position: "bottom"
                    });
                }

            },


        }
    }
</script>
<style lang="scss">
.stage {
  width: 1400px;
  height: 400px;
  position: absolute;
  margin-left: 20px;
  margin-top: 20px;

  }
  .player {
    /*position: absolute;*/
    /*float: left;*/
    width: 1400px;
    margin-left: 20px;
    margin-top: 20px;
    position: fixed;
  }

.demo-control{
  float: right;
  /*position: relative;*/
}



</style>
