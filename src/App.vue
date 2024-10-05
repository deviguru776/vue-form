<template>
<div class="small-container">
<h1>Employees</h1>
<employee-form @add:employee="addEmployee"/> 
<employee-table :employees="employees" @delete:employee="deleteEmployee" />.

</div>
</template>

<script>
import EmployeeForm from './components/Employee form.vue'
import EmployeeTable from './components/EmployeeTable.vue'

export default {
  name: 'App',
  components: {
  EmployeeTable,
    EmployeeForm
  },
  data(){
    return{
      employees:[]
    }
  },
  methods:{
   async addEmployee(employee){

try{
    const response = await  fetch('https://jsonplaceholder.typicode.com/users', 
    {
      method:'POST',
      body:JSON.stringify(employee),
        headers: {
    'Content-type': 'application/json; charset=UTF-8',
  }
      }
      );
const data = await response.json();
      
      this.employees =[...this.employees,data];
   

    }catch(error){

  console.log(error);
  
}
 },


 async deleteEmployee(id){

  try{
await fetch('https://jsonplaceholder.typicode.com/users'+id,{
  
  method:'DELETE'

 })


 this.employees = this.employees.filter((employee)=>{  
      return employee.id !=id; 
    })

  }catch(error){
    console.log(error);
  }
   
  },
 async editemployee(id,updatedemployee){

  try{

      const response =  await  fetch('https://jsonplaceholder.typicode.com/users',+id,
    {
      method:'PUT',
      body:JSON.stringify(updatedemployee),
        headers: {
    'Content-type': 'application/json; charset=UTF-8',
  }
      }
      );
const data = await response.json();

  
     this.employees.map((employee)=>{
      return employee.id !=id?data:employee;
    })

    }catch(error){
console.log(error);

  }

  },
  async getemployee(){

    try{

    const response = await  fetch('https://jsonplaceholder.typicode.com/users', {method:'GET'})
    const data = await response.json();
    this.employees=data;

    } catch(error){

      console.log(error)

    }
  }
  },
  mounted(){
   this.getemployee();
  }
}
</script>
<style>
button {
  background: #34ad38;
  border: 1px solid green;
  font-size: 17px;
  font-style: italic;
  font-family: math;
  margin-top: 20px;
  padding: 10px 15px; /* Add padding for better touch targets */
  width: 102%; /* Make button full-width on smaller screens */
  box-sizing: border-box; /* Ensure padding doesn't affect width */
}

button:hover {
  background-color: #a0f087;
}

.small-container {
  max-width: 680px;
  justify-items: center;
  margin: 0 auto; /* Center the container */
  padding: 15px; /* Add padding for better spacing */
}

h1 {
  font-family:unset;
  font-style: italic;
  text-align: center;
 

}

/* Responsive styles */
@media (max-width: 768px) {
  h1 {
    font-size: 24px; 
  }

  button {
    font-size: 15px;
  }

  .small-container {
    padding: 10px; 
  }
}

@media (max-width: 480px) {
  button {
    font-size: 14px;
  }

  .small-container {
    padding: 5px;
  }
}
</style>

