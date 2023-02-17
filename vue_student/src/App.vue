<template>
<div class="first-head-mom">
  <div class="first-head"> </div>
</div>
<div class="body-mom">
  <div class="left-mom">
  
  </div>
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
      student:[],
      errors:[],
      Id:"",
      Points:0,
      StudentName:"Fedor"
    }
  },
  created(){
    this.GetStudent();
  },
  methods: {
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
}
.first-head-mom{
  display: flex;
  justify-content: center;
}
.first-head{
  width: 100%;
  height: 120px;
  border-radius: 20px;
  background-color: #ffffff;
  color: #1d1617;
  font-family: "Roboto";
  display: flex;
  justify-content: left;
  align-items: center;
  
  padding-bottom: 15px;
  margin-bottom: 15px;
  box-shadow: 0px 0px 13px 7px rgba(34, 60, 80, 0.1);
}
.body-mom{
  display: flex;
  justify-content: left;
}
.body{
  width: 100%;
}
.left-mom{
  width: 70px;
  height: 600px;
  border-radius: 20px;
  background-color: #ffffff;
  color: #1d1617;
  font-family: "Roboto";
  display: flex;
  justify-content: left;
  align-items: center;
  padding-left: 15px;
  padding-right: 15px;
  padding-bottom: 15px;
  margin-right: 15px;
  box-shadow: 0px 0px 13px 7px rgba(34, 60, 80, 0.1);
}
.big-head{
  display: flex;
  justify-content: center;
  
}
.big-student{
  display: flex;
  justify-content: center;
  
  
}
.AddStudent{
  text-align: center;
  text-justify: center;
  width: 50px;
  height: 50px;
  border-radius: 25px;
  margin: 0 0 0 auto;
  background: linear-gradient(45deg, #92a3fd, #9dceff);
  border: 0px;
  
}
.AddStudent:hover {
  background: linear-gradient(45deg, #c58bf2, #eea4ce);
  box-shadow: 0px 0px 10px 5px #e09bda;
}
.head{
  width: 100%;
  max-width: 1920px;
  height: 80px;
  border-radius: 20px;
  background-color: #ffffff;
  color: #1d1617;
  display: flex;
  justify-content: left;
  align-items: center;
  padding-left: 15px;
  padding-right: 15px;
  box-shadow: 0px 0px 13px 7px rgba(34, 60, 80, 0.1);
  
}

.students{
  width: 100%;
  max-width: 1920px;
  background-color: #ffffff;
  border-radius: 20px;
  justify-content: left;
  align-items: center;
  padding-left: 15px;
  padding-right: 15px;
  padding-top: 15px;
  padding-bottom: 15px;
  margin-top: 15px;
  box-shadow: 0px 0px 13px 7px rgba(34, 60, 80, 0.1);
}
.students-item{
  border-radius: 20px;
  padding-left: 15px;
  padding-right: 15px;
  padding-top: 15px;
  padding-bottom: 15px;
  margin-top: 15px;

  display: flex;
  align-items: center;
  
  box-shadow: 0px 0px 10px 2px rgba(34, 60, 80, 0.1);
}
.students-item:hover {
  background-color: #f7f8f8;
}

.students-item_points{
  width: 100px;
  text-align: right;
  
}
.students-item_plus10{
  margin: 0 0 0 auto;
  
}
.students-item_plus10-btn{
  width: 50px;
  height: 30px;
  border-radius: 20px;
  background: linear-gradient(45deg, #92a3fd, #9dceff);
  border: 0px;
  margin-right: 15px;
}
.students-item_plus10-btn:hover {
  box-shadow: 0px 0px 10px 5px #e09bda;
  background: linear-gradient(45deg, #c58bf2, #eea4ce);
}
.deleteStudent{
  width: 30px;
  height: 30px;
  border-radius: 20px;
  background: linear-gradient(45deg, #92a3fd, #9dceff);
  border: 0px;
  margin-left: 15px;
}
.deleteStudent:hover {
  box-shadow: 0px 0px 10px 5px #e09bda;
  background: linear-gradient(45deg, #c58bf2, #eea4ce);
}
.UpdateStudent{
  width:50%;
  height: 600px;
  border-radius: 20px;
  background-color: #ffffff;
  color: #1d1617;
  font-family: "Roboto";
  
  padding-left: 15px;
  padding-right: 15px;
  padding-bottom: 15px;

  margin-top: 15px;
  
  margin-left: 15px;
  box-shadow: 0px 0px 13px 7px rgba(34, 60, 80, 0.1);
}
.editName{
  width: 100%;
  height: 60px;
  margin-top: 30px;
  border-radius: 20px;
  border: 0px;
  box-shadow: 0px 0px 10px 2px rgba(34, 60, 80, 0.1);
  text-indent: 5px;
}

.editPoint{
  width: 100%;
  height: 60px;
  margin-top: 15px;
  border-radius: 20px;
  border: 0px;
  box-shadow: 0px 0px 10px 2px rgba(34, 60, 80, 0.1);
  text-indent: 5px;
}

.saveStudentChangeBtn{
  width: 100%;
  height: 60px;
  margin-top: 15px;
  border-radius: 20px;
  border: 0px;
  background: linear-gradient(45deg, #92a3fd, #9dceff);
}
.saveStudentChangeBtn:hover {
  box-shadow: 0px 0px 10px 5px #e09bda;
  background: linear-gradient(45deg, #c58bf2, #eea4ce);
}
.addStudentBtn{
  width: 100%;
  height: 60px;
  margin-top: 15px;
  border-radius: 20px;
  border: 0px;
  background: linear-gradient(45deg, #92a3fd, #9dceff);
}
.addStudentBtn:hover{
  box-shadow: 0px 0px 10px 5px #e09bda;
  background: linear-gradient(45deg, #c58bf2, #eea4ce);
}

</style>
