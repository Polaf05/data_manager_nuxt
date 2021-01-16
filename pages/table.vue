<template>
    <div>
        <table style="width:100%">
                <tr>
                <th>Name</th>
                <th>Address</th>
                <th>Age</th>
                <th>Monthly Income</th>
                <th>Role</th>
                <th>Sex</th>
                <th>Household Members</th>
                </tr>

                <tr v-for="(users, index) in user" :key="index">
                <td>{{users.name}}</td>
                <td>{{users.address}}</td>
                <td>{{users.age}}</td>
                <td>{{users.income}}</td>
                <td>{{users.role}}</td>
                <td>{{users.sex}}</td>
                <td>{{users.household.length}}</td>
                </tr>
        </table>
    </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/firestore";
import "firebase/auth";

export default {
    
    props:['email'],

    mounted(){
        

        firebase.firestore().collection("admin").where("email","==",this.email).get().then((snapshot) => {
            snapshot.docs.forEach((docs) => {
                this.barangay = docs.data().barangay;
                console.log(this.barangay)
              firebase.firestore().collection("user").where("barangay","==",this.barangay).get().then((snapshot) => {
                    snapshot.docs.forEach((docs) => {

                        if(docs.data().status == 'approved'){
                        this.user = [...this.user, docs.data()];
                            
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
  border: 1px solid rgb(253, 239, 239);
  padding: 8px;
  color:rgb(26, 26, 26);
}
tr:nth-child(even) {
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

</style>