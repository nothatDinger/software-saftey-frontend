<template>
  <q-layout view="lHh Lpr lFf">

    <q-header bordered class="bg-white text-dark">

          <TopBar @send="Send"></TopBar>

    </q-header>

    <q-drawer show-if-above v-model="left" side="left" bordered :width="400" :mini-width="300">
      <div class="side_bar">
        <q-tree class="col-12 col-sm-6"
                :nodes="items"
                node-key="label"
                tick-strategy="leaf"
                no-connectors
                v-model:selected= "selected"
                v-model:ticked="ticked"
                v-model:expanded="expanded"
        />

      </div>

      <q-separator class="q-mb-md" />
      <RunningProcess :running_process="running_process"></RunningProcess>
    </q-drawer>

    <q-drawer show-if-above v-model="right" side="right" bordered elevated>
      <ProcessInfo :process_info="process_info"></ProcessInfo>
      <q-separator class="q-mb-md" />
      <ActionAnalyse :warning_text="warning_text"></ActionAnalyse>
    </q-drawer>

    <q-page-container >
      <MainBody :rows="rows"></MainBody>
      <router-view />
    </q-page-container>


  </q-layout>
</template>

<script>

import ActionAnalyse from '../components/ActionAnalyse'
import MainBody from '../components/MainBody'
import ProcessInfo from '../components/ProcessInfo'
import TopBar from '../components/TopBar'
import RunningProcess from '../components/RunningProcess'


import { defineComponent, ref ,reactive, toRefs} from 'vue'
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
  components: {
    ActionAnalyse,
    MainBody,
    ProcessInfo,
    TopBar,
    RunningProcess
  },
  // method:{
  //   getTicked(ticked){
  //     this.ticked = ticked
  //   },
  //
  //   // handle_msg(data){
  //   //   console.log("收到消息",data)
  //   //   if( data.processInfo.processName !== this.processInfo.processName){
  //   //     this.processInfo = data.processInfo
  //   //     this.rows = data.rows
  //   //     this.warningText = data.warningText
  //   //   }//更换被hook进程
  //   //   else{
  //   //     this.processInfo = data.processInfo
  //   //     this.rows.apply(data.rows)
  //   //     this.warningText.apply(data.warningText)
  //   //   }
  //   // },
  //   // reconnect() {
  //   //   console.log('尝试重连')
  //   //   if (this.lockReconnect || this.maxReconnect <= 0) {
  //   //     return
  //   //   }
  //   //   setTimeout(() => {
  //   //     // this.maxReconnect-- // 不做限制 连不上一直重连
  //   //     this.initWebSocket()
  //   //   }, 60 * 1000)
  //   // },
  //
  //   // websocketonopen() {
  //   //   console.log('WebSocket连接成功', this.socket.readyState)
  //   //   heartCheck.start(this.socket)
  //   //   // this.socket.send('发送数据')
  //   //   this.websocketsend()
  //   // },
  //   // websocketonerror(e) {
  //   //   console.log('WebSocket连接发生错误', e)
  //   //   this.reconnect()
  //   // },
  //   // websocketonmessage(e) {
  //   //   // console.log(e)
  //   //
  //   // },
  //   // websocketclose(e) {
  //   //   console.log('connection closed (' + e.code + ')')
  //   //   this.reconnect()
  //   // },
  //   // websocketsend(data) {
  //   //   this.socket.send(JSON.stringify(data))
  //   // }
  // },
  data(){
    return {
      left: false,
      right: false,
      selected: 'Send',
      ticked: ['HeapCreate','Registry'],
      expanded: ['Registry'],
      mySet: new Set(),
      items: [
        {label: "MessageBox", children:[{label:"MessageBoxA"}, {label:"MessageBoxW"}]},
        {label: "Heap", children: [{label:"HeapCreate"}, {label:"HeapAlloc"}, {label:"HeapDestroy"}]},
        {label: "Socket", children: [{label:"Bind"}, {label:"Send"}, {label:"Connect"}, {label:"socket"},{label:"Recv"}]},
        {label: "File", children: [{label:"CreateFile"}, {label:"ReadFile"}, {label:"WriteFile"}]},
        {label: "Registry", children: [{label:"RegCreateKeyEx"},{label:"RegSetValueEx"},{label:"RegCloseKey"},{label:"RegOpenKeyEx"},{label:"RegDeleteValue"}]},
        {label: "Memcpy", children:[{label:"Memcpy"}]}
      ],
      process_info:{process_name:"testName", process_ID: 11452, process_priority:"testPriority",process_module:["many","and many"]},
      warning_text:[
        {time:"19:00",text:"there is a message alert "},
        {time:"19:02",text:"there is something wrong"},
        {time:"19:03",text:"HeapAllocating !"}
      ],
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
      running_process:[
        {process_name:"svchost.exe",id:"11176"},
        {process_name:"YourPhone.exe",id:"14406"},
        {process_name:"YourPhone.exe",id:"14406"},
        {process_name:"YourPhone.exe",id:"14406"},
        {process_name:"YourPhone.exe",id:"14406"},{process_name:"YourPhone.exe",id:"14406"},{process_name:"YourPhone.exe",id:"14406"},
        {process_name:"YourPhone.exe",id:"14406"},
        {process_name:"YourPhone.exe",id:"14406"},
        {process_name:"YourPhone.exe",id:"14406"},
      ],
      // asrc,
      wsuri: 'ws://127.0.0.1:9002', // ws wss
      lockReconnect: false, // 连接失败不进行重连
      maxReconnect: 5, // 最大重连次数，若连接失败
      socket: null,
      a: 5,
    }
  },
  // setup (props, context) {
  //   // const this = reactive()
  //   const leftDrawerOpen = ref(false)
  //
  //   return {
  //     Send,
  //     leftDrawerOpen,
  //     websocketonmessage,
  //     websocketsend,
  //     websocketonerror,
  //     websocketclose,
  //     websocketonopen,
  //     initWebSocket,
  //     reconnect,
  //     handle_msg,
  //     toggleLeftDrawer () {
  //       leftDrawerOpen.value = !leftDrawerOpen.value
  //     }
  //   }
  // },
  methods:{
    filter(data){
      if(!data.hasOwnProperty("rows")){
        console("error filter error")
        return data
      }
      return data["rows"].filter((index,value,arr)=>{
        return this.mySet.has(value)
      })
    },
    reconnect() {
      console.log('尝试重连')
      if (this.lockReconnect || this.maxReconnect <= 0) {
        return
      }
      setTimeout(() => {
        this.maxReconnect-- // 不做限制 连不上一直重连
        this.initWebSocket()
      }, 10 * 1000)
    },
    websocketonopen()  {
      console.log('WebSocket连接成功', this.socket)
      heartCheck.start(this.socket)
      // this.socket.send('发送数据')
      this.websocketsend()
    },
    websocketonerror(e){
      console.log('WebSocket连接发生错误', e)
      this.reconnect()
    },
    websocketclose (e) {
      console.log('connection closed (' + e.code + ')')
      this.reconnect()
    },
    setFilter(){
      this.ticked.forEach((item)=>{
        switch (item){
          case "MessageBox":{
            this.mySet.add("MessageBoxA")
            this.mySet.add("MessageBoxW")
            break
          }
          case "Heap":{
            this.mySet.add("HeapCreate")
            this.mySet.add("HeapAlloc")
            this.mySet.add("HeapDestroy")
            break
          }
          case "Socket":{
            this.mySet.add("Bind")
            this.mySet.add("Send")
            this.mySet.add("Connect")
            this.mySet.add("socket")
            this.mySet.add("Recv")
            break
          }
          case "Registry":{
            this.mySet.add("RegCreateKeyEx")
            this.mySet.add("RegSetValueEx")
            this.mySet.add("RegCloseKey")
            this.mySet.add("RegOpenKey")
            this.mySet.add("RegDeleteValue")
            break
          }
          default:
            this.mySet.add(item)
        }
      })
    },
    Send(files, successful){
      this.setFilter()
      if(successful === false && files == null){
        console.log("failed to send")
        return
      }
      //let ticked = this.$refs.hook_bar.ticked
      let msg = {filePath: files.path, fileName:files.name,ticked: this.ticked}

      this.websocketsend(msg)
    },
    websocketonmessage(e){
      if( e.data === "ack"){
        console.log(e)
        return
      }
      let data = JSON.parse(e.data)
      console.log('得到响应', e.data)
      console.log('可以渲染网页数据...')
      // 消息获取成功，重置心跳
      if(!data.hasOwnProperty('heart')){
        this.handle_msg(data)
      }
      heartCheck.start(this.socket)
    },
    websocketsend(data){
      // if(this.socket.CLOSED){
      //   reconnect()
      // }
      console.log("send",data)
      this.maxReconnect = 5
      if(this.socket.readyState===3){
        this.reconnect()
      } else {
        this.socket.send(JSON.stringify(data))
      }

    },
    initWebSocket (){
      try {

        if ('WebSocket' in window) {
          this.socket = new WebSocket(this.wsuri)
          console.log('test')
        } else {
          console.log('您的浏览器不支持websocket')
        }
        this.socket.onopen = this.websocketonopen
        this.socket.onerror = this.websocketonerror
        this.socket.onmessage = this.websocketonmessage
        this.socket.onclose = this.websocketclose
        console.log(this.socket)
      } catch (e) {
        this.reconnect()
      }
    },
    handle_msg(data){
      console.log("收到消息",data,this.rows)

      if( !data.hasOwnProperty("process_info")&&!data.hasOwnProperty("rows") ){
        console.log("not i WANT", data,data.process_info,data.rows)
      }
      if(data.hasOwnProperty("running_process")){
        this.running_process = data.running_process
      }
      if( data.hasOwnProperty("process_info")&&data.process_info.process_name !== this.process_info.process_name){
        this.process_info = data['process_info']

        if(data.hasOwnProperty("rows")) {
          this.rows = this.filter(data.rows)
        }else{
          this.rows = []
        }
        if(data.hasOwnProperty("warning_text")){
          this.warning_text = data['warning_text']
        }else{
          this.rows = []
        }
      }//更换被hook进程
      else{
        //this.process_info = data['process_info']
        if(data.hasOwnProperty("rows")){
          this.rows.push(...this.filter(data.rows))
        }
        if(data.hasOwnProperty("warning_text")){
          this.warning_text.push(...data['warning_text'])
        }
        this.ticked = []
        this.selected= []
        this.expanded= []
      }


    }
  },
  mounted() {
    console.log(this.rows)
    //socket初始化
    this.initWebSocket()
  },
  unmounted() {
    console.log("closed")
    this.socket.close()
  }
})
</script>
