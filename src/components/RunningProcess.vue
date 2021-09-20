<template>
  <div class="running_process">
    <div class = "q-ma-md">
    <q-table
      title="Process Running in Your Device"
      :rows="rows"
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
const rows=[
  {process_name:"svchost.exe",id:"11176"},
  {process_name:"YourPhone.exe",id:"14406"},
  {process_name:"YourPhone.exe",id:"14406"},
  {process_name:"YourPhone.exe",id:"14406"},
  {process_name:"YourPhone.exe",id:"14406"},{process_name:"YourPhone.exe",id:"14406"},{process_name:"YourPhone.exe",id:"14406"},
  {process_name:"YourPhone.exe",id:"14406"},
  {process_name:"YourPhone.exe",id:"14406"},
  {process_name:"YourPhone.exe",id:"14406"},



]

const pagination = {
  sortBy: 'id',
  descending: false,
  rowsPerPage: 8
}
export default {
  name: "RunningProcess",
  setup(){
    return {
      columns,
      rows,
      pagination
    }
  }
}


</script>


<style scoped>


</style>
