<template>
     <div class="employee-form">
          <form v-on:submit.prevent="handleSubmit">
               <label>Employee name</label>
               <input type="text" v-model="employee.name" :class="{'has-error':submitting&&invalidName}" @focus="clearStatus" @keypress="clearStatus" ref="firstInput"/>
                <label>Employee email</label>
               <input type="text" v-model="employee.email" :class="{'has-error':submitting&&invalidEmail}" @focus="clearStatus" @keypress="clearStatus"/> 

               <p v-if="submitting && error" class="error-message">Please fill out all the required fields!</p>
               <p v-if="success" class="success-message">Employee successfully added!</p>

               <button>Add employee</button>
          </form>
     </div>
</template>
<script>
export default {
     name :'employee-form',
     data(){
          return {
               submitting:false,
               success:false,
               error:false,
               employee:{
                    name :'',
                    email :''
               }
          }
     },

     methods:{
          handleSubmit(){
                this.submitting =true;
               this.clearStatus();

               if(this.invalidName||this.invalidEmail){
                    this.error=true;
                    return;
               }

               this.$emit('add:employee',this.employee);
               this.$refs.firstInput.focus();
               this.employee ={
                    name :'',
                    email :''
             };
              this.success=true;
              this.error=false;
              this.submitting=false;
          },
          clearStatus(){
                this.success=false;
                this.error=false;
          }
     },
     computed:{
            invalidName(){
               return this.employee.name==='';
          },
          invalidEmail(){
               return this.employee.email==='';
          }
     }

}

</script>
<style scoped>
.add-employee {
    margin: 40px 0;
    border: 1px solid #ddd;
    border-radius: 5px; 
    background-color: #f9f9f9;
}

label {
    display: block;
    margin: 10px 0 5px;
    font-size: 17px;
    font-style: italic;
    font-weight: 700;
}

input {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-bottom: 10px;
}

.has-error{
     border-color: red;
}


button {
    background-color: #34ad38;
    color: rgb(12, 12, 12);
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: 500;
}

button:hover {
    background-color: #589ff0;
}

.error-message{
     color: red;
}

.success-message{
     color:green;
}
</style>