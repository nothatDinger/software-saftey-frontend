<template>
  <q-layout view="lHh Lpr lFf">

    <q-header bordered class="bg-white text-dark">

          <TopBar @send="Sendss"></TopBar>

    </q-header>

    <q-drawer show-if-above v-model="left" side="left" bordered>
      <HookBar ref="hook_bar"></HookBar>
      <q-separator class="q-mb-md" />
      <RunningProcess></RunningProcess>
    </q-drawer>

    <q-drawer show-if-above v-model="right" side="right" bordered elevated>
      <ProcessInfo :process-info="processInfo"></ProcessInfo>
      <q-separator class="q-mb-md" />
      <ActionAnalyse :warning-text="warningText"></ActionAnalyse>
    </q-drawer>

    <q-page-container >
      <MainBody :rows="rows"></MainBody>
      <router-view />
    </q-page-container>


  </q-layout>
</template>

<script>

import ActionAnalyse from '../components/ActionAnalyse'
import HookBar from '../components/HookBar'
import MainBody from '../components/MainBody'
import ProcessInfo from '../components/ProcessInfo'
import TopBar from '../components/TopBar'
import RunningProcess from '../components/RunningProcess'


import { defineComponent, ref } from 'vue'
const heartCheck = {
  timeout: 60 * 1000,
  timer: null,
  serverTimer: null,
  reset() {
    this.timer && clearTimeout(this.timer)
    this.serverTimer && clearTimeout(this.serverTimer)
  },
  start(ws) {
    this.reset()
    this.timer = setTimeout(() => {
      // console.log('发送心跳,后端收到后，返回一个心跳消息')
      // onmessage拿到返回的心跳就说明连接正常
      ws.send(JSON.stringify({ heart: 1 }))
      this.serverTimer = setTimeout(() => {
        // 如果超过一定时间还没响应(响应后触发重置)，说明后端断开了
        ws.close()
      }, this.timeout)
    }, this.timeout)
  }
}
export default defineComponent({
  name: 'MainLayout',
  data () {
    let processInfo;
    let warningText;
    let rows;
    return {
      left: false,
      right: false,
      ticked: ref([]),
      processInfo:{processName:"testName", processID: 11452, processPriority:"testPriority",processModule:["many","and many"]},
      warningText:[{time:"19:00",text:"there is a message alert "},{time:"19:02",text:"there is something wrong"},{time:"19:03",text:"HeapAllocating !"}],
      rows:[
        {name:"MessageBoxA", time:"16:10",args:[{argName:"test",argValue:"test"},{argName:"test2",argValue:"test2"},{argName:"test3",argValue:"test3"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]},
        {name:"HeapCreate", time:"17:20",args:[{argName:"test4",argValue:"test4"},{argName:"test5",argValue:"test5"},{argName:"test6",argValue:"test6"}]}
      ],
      asrc,
      wsuri: 'ws://127.0.0.1:9002', // ws wss
      lockReconnect: false, // 连接失败不进行重连
      maxReconnect: 5, // 最大重连次数，若连接失败
      socket: null
    }
  },
  components: {
    ActionAnalyse,
    HookBar,
    MainBody,
    ProcessInfo,
    TopBar,
    RunningProcess
  },
  method:{
    getTicked(ticked){
      this.ticked = ticked
    },
    Sendss(files, successful){
      if(successful === false && files == null){
        console.log("failed to send")
        return
      }
      let ticked = this.$refs.hook_bar.ticked
      let msg = {file: files, ticked: ticked}
      this.websocketsend(msg)
    },
    handle_msg(data){
      console.log("收到消息",data)
      if( data.processInfo.processName !== this.processInfo.processName){
        this.processInfo = data.processInfo
        this.rows = data.rows
        this.warningText = data.warningText
      }//更换被hook进程
      else{
        this.processInfo = data.processInfo
        this.rows.apply(data.rows)
        this.warningText.apply(data.warningText)
      }
    },
    reconnect() {
      console.log('尝试重连')
      if (this.lockReconnect || this.maxReconnect <= 0) {
        return
      }
      setTimeout(() => {
        // this.maxReconnect-- // 不做限制 连不上一直重连
        this.initWebSocket()
      }, 60 * 1000)
    },
    initWebSocket() {
      try {
        if ('WebSocket' in window) {
          this.socket = new WebSocket(this.wsuri)
        } else {
          console.log('您的浏览器不支持websocket')
        }
        this.socket.onopen = this.websocketonopen
        this.socket.onerror = this.websocketonerror
        this.socket.onmessage = this.websocketonmessage
        this.socket.onclose = this.websocketclose
      } catch (e) {
        this.reconnect()
      }
    },
    websocketonopen() {
      console.log('WebSocket连接成功', this.socket.readyState)
      heartCheck.start(this.socket)
      // this.socket.send('发送数据')
      this.websocketsend()
    },
    websocketonerror(e) {
      console.log('WebSocket连接发生错误', e)
      this.reconnect()
    },
    websocketonmessage(e) {
      // console.log(e)
      let data = JSON.parse(e.data)
      console.log('得到响应', data)
      console.log('可以渲染网页数据...')
      // 消息获取成功，重置心跳
      this.handle_msg(e.data)
      heartCheck.start(this.socket)
    },
    websocketclose(e) {
      console.log('connection closed (' + e.code + ')')
      this.reconnect()
    },
    websocketsend(data) {
      this.socket.send(JSON.stringify(data))
    }
  },

  setup () {
    const leftDrawerOpen = ref(false)

    return {
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  },
  mounted() {
    this.initWebSocket()
  },
  unmounted() {
    this.socket.close()
  }
})
</script>
