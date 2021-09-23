<template>
  <div class="running_process">
    <div class = "q-ma-md">
    <q-table
      title="Process Running in Your Device"
      :rows="running_process"
      :columns="columns"
      :pagination="pagination"
      row-key="id"
    ></q-table>
  </div>
  </div>
</template>


<script>

const columns = [
  {name:"Process", label: "Process Name",align:"left",field: row => row.process_name, sortable: true},
  {name:"ID",label:"Process ID", align:"center",field: 'id', sortable:true}
]


const pagination = {
  sortBy: 'id',
  descending: false,
  rowsPerPage: 20
}
export default {
  name: "RunningProcess",
  props:["running_process"],
  setup(){
    return {
      columns,
      pagination
    }
  }
}


</script>


<style scoped>


</style>
