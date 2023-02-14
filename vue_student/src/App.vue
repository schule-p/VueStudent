<template>
  <div class="head">
    <h2>Байтиксы студентов</h2>
  </div>

  <div class="students">
    <div class="students-item" v-for="value in student" :key="value.student">
    <div class="students-item_name">
      <h3>{{ value.studentName }}</h3>
    </div>
    <button class="students-item_plus10">+10B</button>
    <span class="students-item_points">{{value.points}}</span>
    </div>
  </div>

</template>

<script>
import axios from 'axios'

export default{
  name: 'GetStudents',
  data(){
    return{
      student:[],
      errors:[]
    }
  },
  created(){
    axios.get('https://localhost:5001/api/Student/GetStudents')
    .then(responce => {
      this.student = responce.data
      console.log(responce)
    })
    
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding-top: 40px;
  padding-left: 30px;
  padding-right: 30px;
}
.head{
  width: 100%;
  max-width: 800px;
  height: 80px;
  border-radius: 20px;
  background-color: #ffffff;
  color: #1d1617;
  font-family: "Roboto";
  display: flex;
  justify-content: left;
  align-items: center;
  padding-left: 15px;
  padding-right: 15px;
}

.students{
  width: 100%;
  max-width: 800px;
  background-color: #ffffff;
  border-radius: 20px;
  justify-content: left;
  align-items: center;
  padding-left: 15px;
  padding-right: 15px;
  padding-top: 15px;
  padding-bottom: 15px;
  margin-top: 15px;
}
.students-item{
  
  border: 1px solid #c58bf2;
  border-radius: 20px;
  padding-left: 15px;
  padding-right: 15px;
  padding-top: 15px;
  padding-bottom: 15px;
  margin-top: 15px;

  display: flex;
  align-items: center;
  
}
.students-item_name{
  margin-right: 15px;
}
.students-item_points{
  flex: 0 0 80px;
  text-align: right;
  margin-right: 0px;
  

}
.students-item_plus10{
  width: 50px;
  height: 30px;
  border-radius: 20px;
  border: 1px solid #c58bf2;
  background-color: #eea4ce;
  margin-right: 15px;
  
  
  
}



</style>
