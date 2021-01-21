<template>
    <div>
        <span> Filter by: </span> <br/>
        <select  class= "select" v-model="select_model">
            <option value="Family">FAMILY REPRESENTATIVES</option>
            <option value="Senior">SENIORS</option>
            <option value="Student">STUDENT</option>
            <option value="Employed">EMPLOYED</option>
            <option value="Unemployed">UNEMPLOYED</option>
            <option value="All">All</option>
        </select>
        <div v-if="select_model == 'Family'">
            <h2 align="center">FAMILY REPRESENTATIVES</h2>
            <table style="width:100%">
                    <tr>
                    <th>Name</th>
                    <th>Address</th>
                    <th>Age</th>
                    <th>Monthly Income</th>
                    <th>Sex</th>
                    </tr>

                    <tr v-for="(users, index) in user" :key="index">
                    <td>{{users.name}}</td>
                    <td>{{users.address}}</td>
                    <td>{{users.age}}</td>
                    <td>{{users.income}}</td>
                    <td>{{users.sex}}</td>
                    
                    </tr>
            </table>
        </div>

        <div v-if="select_model == 'Unemployed'">
            <h2 align="center">UNEMPLOYED</h2>
            <table style="width:100%">
                    <tr>
                    <th>Name</th>
                    <th>Address</th>
                    <th>Age</th>
                    <th>Sex</th>
                    </tr>
                
                    <tr v-for="(users, index) in all" :key="index">
                     
                    <td v-if="users.employment == 'Unemployed'">{{users.name}}</td>
                    <td v-if="users.employment == 'Unemployed'">{{users.address}}</td>
                    <td v-if="users.employment == 'Unemployed'">{{users.age}}</td>
                    <td v-if="users.employment == 'Unemployed'">{{users.sex}}</td>
                    
                    </tr>
                   
            </table>
        </div>

        <div v-if="select_model == 'Employed'">
           <h2 align="center">EMPLOYED</h2>
            <table style="width:100%">
                    <tr>
                    <th>Name</th>
                    <th>Address</th>
                    <th>Age</th>
                    <th>Sex</th>
                    </tr>
                
                    <tr v-for="(users, index) in all" :key="index">
                     
                    <td v-if="users.employment == 'Employed'">{{users.name}}</td>
                    <td v-if="users.employment == 'Employed'">{{users.address}}</td>
                    <td v-if="users.employment == 'Employed'">{{users.age}}</td>
                    <td v-if="users.employment == 'Employed'">{{users.sex}}</td>
                    
                    </tr>
                   
            </table>
        </div>

        <div v-if="select_model == 'Senior'">
            <h2 align="center">SENIORS</h2>
            <table style="width:100%">
                    <tr>
                    <th>Name</th>
                    <th>Address</th>
                    <th>Age</th>
                    <th>Sex</th>
                    </tr>
                
                    <tr v-for="(users, index) in all" :key="index">
                     
                    <td v-if="users.employment == 'Senior'">{{users.name}}</td>
                    <td v-if="users.employment == 'Senior'">{{users.address}}</td>
                    <td v-if="users.employment == 'Senior'">{{users.age}}</td>
                    <td v-if="users.employment == 'Senior'">{{users.sex}}</td>
                    
                    </tr>
                   
            </table>
        </div>

        
        <div v-if="select_model == 'Student'">
            <h2 align="center">STUDENTS</h2>
            <table style="width:100%">
                    <tr>
                    <th>Name</th>
                    <th>Address</th>
                    <th>Age</th>
                    <th>Sex</th>
                    </tr>
                
                    <tr v-for="(users, index) in all" :key="index">
                     
                    <td v-if="users.employment == 'Student'">{{users.name}}</td>
                    <td v-if="users.employment == 'Student'">{{users.address}}</td>
                    <td v-if="users.employment == 'Student'">{{users.age}}</td>
                    <td v-if="users.employment == 'Student'">{{users.sex}}</td>
                    
                    </tr>
                   
            </table>
        </div>

        <div v-if="select_model == 'All'">
            <h2 align="center">ALL</h2>
            <table style="width:100%">
                    <tr>
                    <th>Name</th>
                    <th>Address</th>
                    <th>Age</th>
                    <th>Sex</th>
                    </tr>
                
                    <tr v-for="(users, index) in all" :key="index">
                     
                    <td>{{users.name}}</td>
                    <td>{{users.address}}</td>
                    <td>{{users.age}}</td>
                    <td>{{users.sex}}</td>
                    
                    </tr>
                   
            </table>
        </div>
    </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/firestore";
import "firebase/auth";

export default {
    
    props:['email'],

    mounted(){
        
        console.log(this.email)
        firebase.firestore().collection("admin").where("email","==",this.email).get().then((snapshot) => {
            snapshot.docs.forEach((docs) => {
                this.barangay = docs.data().barangay;
                console.log(this.barangay)
              firebase.firestore().collection("user").where("barangay","==",this.barangay).get().then((snapshot) => {
                    snapshot.docs.forEach((docs) => {

                        if(docs.data().status == 'approved'){
                        this.user = [...this.user, docs.data()];
                        this.all = [...this.all, docs.data()];
                        }
  
                    });
                });
              firebase.firestore().collection("members").where("barangay","==",this.barangay).get().then((snapshot) => {
                    snapshot.docs.forEach((docs) => {

                        if(docs.data().status == 'approved'){
                        this.all = [...this.all, docs.data()];
                            
                        }
  
                    });
                });
          
            });
        });
 

    },


    data() {

        return {
        barangay:'',
        user: [],
        all:[],
        select_model:'Family',

        };

    },
}

</script>

<style scope>

*{
    margin:0;
    padding:0;
}

table {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}
td,
th {
  border: 1px solid rgb(151, 151, 151);
  padding: 8px;
  color:rgb(26, 26, 26);
}
tr {
  background-color: #f2f2f2;
}
tr:hover {
  background-color: #ddd;
}
th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #3d3f3f;
  color: white;
}
.select{
   background-color: #35495e;
   color: white;
   padding: 5px;
   width: 250px;
   height: 50px;
}

</style>