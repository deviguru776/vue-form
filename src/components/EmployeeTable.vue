<template>
<div id="employee-Table">

     <p v-if="employees.length<1" class="empty-table">No Employees</p>
     <table v-else>
          <thead>
               <tr>
                    <th>Employee name</th>
                    <th>Employee email</th>
                     <th>Action</th>
               </tr>
          </thead>
          <tbody>
               <tr v-for="employee in employees" :key="employee.id">
                    <td v-if="editting==employee.id">
                         <input type="text" v-model="employee.name">
                    </td>

                    <td v-else>{{employee.name}}</td>

                    <td v-if="editting==employee.id">
                         <input type="text" v-model="employee.email">
                    </td>
                    <td v-else>{{employee.email}}</td>
                         <td v-if="editting==employee.id">
                              <button @click="editemployee(employee)">Save</button>
                               <button class="muted-button" @click="canceledit(employee)">Cancel</button>
                         </td>
                    <td v-else>
                         <button @click="editMode(employee)">Edit</button>
                         <button @click="$emit('delete:employee',employee.id)">Delete</button>
                    </td>
               </tr>
          </tbody>
     </table>
</div>
</template>
<script>
export default {
     name : 'employee-table',
     props:{
          employees:Array
     },
     data(){
          return{
               editting:null,
               employee:{
                     name :'',
                    email :''
               },
               cachedemployee:null
          }
     },
     methods:{
          editMode(employee){
               this.editting =employee.id;
               this.cachedemployee = Object.assign({},employee)
          },
          editemployee(employee){
               if(employee.name==''||employee.email==''){
                    return;
               }
               this.$emit('edit:employee',employee.id,employee);
               this.editting=null;
          },
          canceledit(employee){
               this.editting=null;
               Object.assign(employee,this.cachedemployee);
          }
     }
}
</script>
<style scoped>
.container {
    max-width: 800px;
    margin: auto;
    background: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  
}

h1, h2 {
    color: #333;
    text-align: center;
}

table {
    width: 102%;
    border-collapse: collapse;
    margin-bottom: 20px;
    margin-top: 34px;
}

th, td {
    padding: 10px;
    border: 1px solid #ddd;
    font-size: 17px;
    font-style: italic;
    font-weight: 400;
  
}

th {
    background-color: #f2f2f2;
    font-size: 17px;
    font-style: italic;
    font-weight: 600;
}

button{
     margin: 7px 0.5rem 6px 0;
     border-radius: 5px;
}

.empty-table{
     text-align: center;
     font-size: 23px;
     font-style: italic;
     font-weight: 700;
     color: red;
}

.muted-button{
     background-color: aliceblue;
}
</style>