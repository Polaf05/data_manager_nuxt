<template>
    <div>
        <table style="width:100%">
                <tr>
                <th>Name</th>
                <th>Status</th>
                <th>Valid Id</th>
                </tr>

                <tr v-for="(users, index) in user" :key="index">
                <td>{{users.name}}</td>
                <td>
                    {{users.status}}
                    <b-dropdown id="dropdown-1" class="m-md-2">
                    <b-dropdown-item @click="statusChange('approved',users.email,index)" >Approve</b-dropdown-item>
                    <b-dropdown-item @click="statusChange('decline',users.email,index)" >Decline</b-dropdown-item>
                    </b-dropdown>
                </td>
                <td><a :href="users.image" target="_POST">check id</a></td>
                
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

                        if(docs.data().status == 'pending'){
                        this.user = [...this.user, docs.data()];
                        }
  
                    });
                });
          
            });
        });
 

    },

    methods:
    {
        statusChange(status, email,index){
            this.hide = true
        var id=""
        firebase.firestore().collection('user').where("email","==",email).get().then(snapshot => {
            snapshot.docs.forEach(docs => {
                id = docs.id
                this.user[index].status = status
                firebase.firestore().collection("user").doc(id).update({
                    status: status
                }).then(function() {
                console.log("Document successfully updated!");
                
                })
                .catch(function(error) {
                    console.error("Error updating document: ", error);
                });

                firebase.firestore().collection('members').where("email","==",email).get().then(snapshot => {
                    snapshot.docs.forEach(docs => {
                        id = docs.id
                        this.user[index].status = status
                        firebase.firestore().collection("members").doc(id).update({
                        status: status
                    }).then(function() {
                    console.log("Document successfully updated!");
                    
                    })
                    .catch(function(error) {
                        console.error("Error updating document: ", error);
                    });
                            })
                        }).catch(function(error) {
                        console.log("Error getting documents: ", error);
                    });


                    })
                }).catch(function(error) {
                console.log("Error getting documents: ", error);
            });
      
    }

    },


    data() {

        return {
        barangay:'',
        user: [],
        hide:false,
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