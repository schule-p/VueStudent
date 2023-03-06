<template>
<div class="global">
<div class="first-head-mom">
  <div class="first-head"> 
    <div id="app">
      <test></test>
    </div>
    
  </div>
</div>
<div class="body-mom">
  <div class="left-mom"></div>

  <div class="body">
    <div class="big-head">
      <div class="head">
        <h2>Байтиксы студентов</h2>
      </div>
    </div>
      
    <div class="big-student">
      <div class="students">
          <div class="students-item"  v-for="value in student" :key="value.student" @click="EditStudent(value)">
            <div class="students-item_name">
              <h4>{{ value.studentName }}</h4>
            </div>
            <div class="students-item_plus10">
              <button class="students-item_plus10-btn" @click="Plus10PointsStudent(value.id)">+10B</button>
              <span class="students-item_points">{{value.points}}</span>
              <button class="deleteStudent" @click="DeleteStudent(value.id)">×</button>
            </div>
          </div>
        
      </div>
      <div class="UpdateStudent">
        <input class="editName" v-model="Name" placeholder="Имя"> <h4>{{ studentName }}</h4>
        <input class="editPoint" v-model="StudentsPoints" placeholder="Байтики"> <h4>{{ points }}</h4>
        <button class="saveStudentChangeBtn" @click="UpdateStudent()"><h4>Сохранить</h4></button>
        <button class="addStudentBtn" @click="AddStudent"><h4>Добавить</h4></button>
        


        <input class="editName" v-model="Name" placeholder="Имя"> <h4>{{ studentName }}</h4>
        <input class="editPoint" v-model="StudentsPoints" placeholder="Байтики"> <h4>{{ points }}</h4>

        
        <div class="UpDownBaitics">
          <input type="radio" id="one" value="Начисление" v-model="picked" />
          <label for="Начисление"> Начисление </label>

          <input type="radio" id="two" value="Списание" v-model="picked" />
          <label for="Списание"> Списание </label>

          <button class="addStudentBtn" @click="AddTransactions"><h4>Добавить</h4></button>
        </div>
        

      </div>
    </div>
    <div class="transactions">
      <input class="searchTransaction" v-model="transName" placeholder="Имя">
      <div class="transaction-item" v-for="trans in filtredTransactions"  :key="trans" >
          <h4 class="students-item_name">{{ trans.student.studentName }}</h4>
          <div class="trans-item_right">
            <h4 class="statusTransN" v-if="trans.typeOfTransaction">+{{ trans.sum }} Б</h4>
            <h4 class="statusTransS" v-else>-{{ trans.sum }} Б</h4>
          </div>
      </div>
    </div>
  </div>
</div>
</div>
</template>

<script>
import axios from 'axios'

console.log(axios)

export default{
  name: 'GetStudents',
  data(){
    return{
      picked: 'Начисление',
      transName:"",
      student:[],
      transactions:[],
      info:[],
      errors:[],
      Id:"",
      Points:0,
      StudentName:"Fedor"
    }
  },
  created(){
    this.GetStudent();
    this.GetTransaction();
  },
  methods: {
    async GetTransaction(){
      await axios.get('https://localhost:7186/api/Transaction/GetTransactions')
      .then(responce => {
      this.transactions = responce.data
    })
    },

    async GetStudent(){
      await axios.get('https://localhost:7186/api/Student/GetStudents')
    .then(responce => {
      this.student = responce.data
    })
    },
    EditStudent(value){
      this.Name = value.studentName;
      this.StudentsPoints = value.points;
      this.EditId = value.id;
      //this.EditId = 4;
      this.GetStudent();
    },
    async UpdateStudent(){
      let date = new Date();
      await axios.put("https://localhost:7186/api/Student/UpdateStudent/"+this.EditId,{
        id: this.EditId,
        studentName: this.Name,
        points: this.StudentsPoints,
        lastDateUpdatePoints: date
      })
      .then(function(res){
        console.log(res)
      }).catch(function(error){
        console.log(error)
      })
      this.GetStudent();
    },
    async Plus10PointsStudent(id) {
      let date = new Date();
      await axios.put("https://localhost:7186/api/Student/Plus10PointsStudent/"+id,{
        id: 0,
        studentName: "string",
        points: 0,
        lastDateUpdatePoints: date
      })
      .then(function(res){
        console.log(res)
      }).catch(function(error){
        console.log(error)
      })
      this.GetStudent();
    },
    async AddStudent(){
      if (this.Points === "" || this. StudentName === ""){
        return false
      }
      
      await axios.post("https://localhost:7186/api/Student/AddStudent",{
        StudentName: this.Name,
        Points: this.StudentsPoints
      }).then(function(res){
        console.log(res)
      }).catch(function(error){
        console.log(error)
      })

      this.GetStudent();

      this.Name = "";
      this.StudentsPoints = "";
    },
    async DeleteStudent(id){
      await axios.delete("https://localhost:7186/api/Student/DeleteStudent/"+id)
      .then(function(res){
        console.log(res)
      }).catch(function(error){
        console.log(error)
      })

      this.GetStudent();
    },
    async AddTransactions(){
      let date = new Date();
      let status = true;
      if (this.picked == "Начисление") {
        status = true;
      } else {
        status = false; 
      }
      await axios.post("https://localhost:7186/api/Transaction/",{
        id: 0,
        sum: this.StudentsPoints,
        typeOfTransaction: status,
        dateTransartion: date,
        idStudent: this.EditId,
        student: {
          id: 0,
          studentName: "string"}
        }).then(function(res){
            console.log(res)
        }).catch(function(error){
        console.log(error)
        })
      this.GetTransaction();
    }
  },
  computed:{
    filtredTransactions(){
      return this.transactions.filter(elem => {
        return elem.student.studentName.toUpperCase().includes(this.transName.toUpperCase());
      });
    }
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
  margin-left: auto;
  margin-right: auto;
  max-width: 1000px;
}
</style>
