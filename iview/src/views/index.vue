<style scoped>
    .index {
        width: 100%;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        text-align: center;
    }

    .index h1 {
        height: 150px;
    }

    .index h1 img {
        height: 100%;
    }

    .index h2 {
        color: #666;
        margin-bottom: 200px;
    }

    .index h2 p {
        margin: 0 0 50px;
    }

    .index .ivu-row-flex {
        height: 100%;
    }
</style>
<template>
    <div class="index">
        <Row type="flex" justify="center" align="middle">
            <Col span="24">
                <h1>
                    <img src="https://raw.githubusercontent.com/iview/iview/master/assets/logo.png">
                </h1>
                <h2>
                    <p>springboot websocket connection!</p>
                    <Input v-model="sendValue" placeholder="Enter Text..." style="width: 300px"></Input>
                    <Button type="ghost" @click="websocket">发送</Button>
                    <br>
                </h2>
               <Input v-model="value5" type="textarea" :autosize="true" placeholder="Enter something..."></Input>
            </Col>
        </Row>
    </div>
</template>
<script>
    export default {
             data () {
                return {
                    sendValue: '',
                    value5:'',
                }
        },
        methods: {
            websocket () {
                 const agentData = this.sendValue;
                //若是ws开启状态
                if (this.websock.readyState === this.websock.OPEN) {
                    this.websocketsend(agentData)
                }
                // 若是 正在开启状态，则等待300毫秒
                else if (this.websock.readyState === this.websock.CONNECTING) {
                    let that = this;//保存当前对象this
                    setTimeout(function () {
                        that.websocketsend(agentData)
                    }, 300);
                }
                // 若未开启 ，则等待500毫秒
                else {
                    this.initWebSocket();
                    let that = this;//保存当前对象this
                    setTimeout(function () {
                        that.websocketsend(agentData)
                    }, 500);
                }
         },
          initWebSocket(){ //初始化weosocket
                //ws地址
                const wsuri = "ws://localhost:8888/webSocket";
                this.websock = new WebSocket(wsuri);
                this.websock.onmessage = this.websocketonmessage;
                this.websock.onclose = this.websocketclose;
            },
            websocketonmessage(e){ //数据接收
                const redata = e.data;
                this.$Message.info(redata);
                this.value5+= redata+"\r\n"
            },
            websocketsend(agentData){//数据发送
                this.websock.send(agentData);
            },
            websocketclose(e){  //关闭
                console.log("connection closed (" + e.code + ")");
            }
        },
        created(){
            this.initWebSocket()
        }
    };
</script>