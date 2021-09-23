<template>

</template>


<script>
import { ionChevronUp } from '@quasar/extras/ionicons-v5'
import { ref ,reactive, toRefs} from 'vue'
export default {
  name: 'HookBar',
  created(){
    this.ionChevronUp = ionChevronUp
  },
  data(){
    return {
      selected: ref('Send'),
      ticked: ref(['HeapCreate','Registry']),
      expanded: ['Registry'],
      items: [
        {label: "MessageBox", children:[{label:"MessageBoxA"}, {label:"MessageBoxW"}]},
        {label: "Heap", children: [{label:"HeapCreate"}, {label:"HeapAlloc"}, {label:"HeapDestroy"}]},
        {label: "Socket", children: [{label:"Bind"}, {label:"Send"}, {label:"Connect"}, {label:"socket"},{label:"Recv"}]},
        {label: "File", children: [{label:"CreateFile"}, {label:"ReadFile"}, {label:"WriteFile"}]},
        {label: "Registry", children: [{label:"RegCreateKeyEx"},{label:"RegSetValueEx"},{label:"RegCloseKey"},{label:"RegOpenKeyEx"},{label:"RegDeleteValue"}]},
        {label: "Memcpy", children:[{label:"Memcpy"}]}
      ]
    }
  },
  methods:{
    return_ticked (){
      context.$emit('return_val', this.ticked)
    }
  },
  // setup(props, context){
  //   const this = reactive()
  //
  //
  //   return{
  //     return_ticked,
  //     ...toRefs(this)
  //   }
  // }
}
</script>

<style scoped>



</style>
