<template>
    <section class="login">
            <div class="loginContainer">
              <form v-on:submit.prevent="uploadFile">
                <h1>Online Barangay Registration</h1> 
                
                <label>Name</label>
                <input type="text" autoFocus required placeholder="Enter Name" v-model="name"/>

                <label>Email</label>
                <input type="text" autoFocus required placeholder="Enter Email Address" v-model="email"/>

                <label>Address</label>
                <input type="text" autoFocus required placeholder="Enter Address" v-model="address"/>

                <label>Age</label>
                <input type="text" autoFocus required placeholder="Enter Age" v-model="age"/>

                <label>Sex</label>
                <select v-model="sex">
                    <option value="Male">Male</option>
                    <option value="Female">Female</option>  
                </select>

                <div v-if="age< 60">
                        <label>Employment</label>
                        <select v-model="employment">
                            <option value="Student">Student</option>
                            <option value="Employed">Employed</option>
                            <option value="Unemployed">Unemployed</option>
                        </select>
                    </div>

                <label>Monthly Income</label>
                <select v-model="income" place>
                    <option value="Below 15,000">Below 15,000</option>
                    <option value="15,000 - 30,000">15,000 - 30,000</option>
                    <option value="30,000 - 50,000">30,000 - 50,000</option>
                    <option value="50,000 above">50,000 above</option>
                </select>

                <div class= 'border'>
                <label>Household Members</label>
                
                    <input type="text" autoFocus placeholder="Enter Member" v-model="member_model"/>
                    
                    <label>Age</label>
                    <input type="text" autoFocus placeholder="Enter Age" v-model="age_household_model"/>
                    
                    <div v-if="age_household_model < 60">
                        <label>Employment</label>
                        <select v-model="employment_household_model">
                            <option value="Student">Student</option>
                            <option value="Employed">Employed</option>
                            <option value="Unemployed">Unemployed</option>
                        </select>
                    </div>

                    <label>Sex</label>
                    <div class='list'>
                        <select v-model="sex_household_model">
                            <option value="Male">Male</option>
                            <option value="Female">Female</option>  
                        </select>
                        <button @click="addList">V</button>
                    </div>

                    <client-only>
                    <table style="width:100%; margin: .2rem;">
                          <tr>
                          <th>Name</th>
                          <th>Status</th>
                          <th>Sex</th>
                          <th>Age</th>
                          <th>Remove</th>
                          </tr>
                          <tr v-for="(member,index) in members" :key="index" class="displayList">
                          <td>{{member}}</td>
                          <td>{{employment_household[index]}}</td>
                          <td>{{sex_household[index]}}</td>
                          <td>{{age_household[index]}}</td>
                          <td><a @click="removeIndex(member, employment_household[index],sex_household[index])" class="hypeLink">Remove</a></td>
                          </tr>
                    </table>
                    </client-only>
                    
                </div>

                <div class="btnContainer">
                   <input type="submit" value="Register">
                </div>
              </form>
            </div>

        </section>
</template>

<script>

import firebase from 'firebase/app'
import 'firebase/auth'
import 'firebase/firestore'
import 'firebase/storage'

export default {

    props:['admin'],

    mounted() {
        
        firebase.firestore().collection("admin").where("email","==",this.admin).get().then((snapshot) => {
            snapshot.docs.forEach((docs) => {
                this.barangay = docs.data().barangay;
                this.barangay_id = docs.id;
            
                });
        });
    },

    methods:{

        processFile(event) {
            this.file = event.target.files[0]
            console.log(this.file);
        },

         uploadFile(){

            firebase.firestore().collection("user").add({
                            name: this.name,
                            address:this.address,
                            age:this.age,
                            sex:this.sex,
                            employment:this.employment,
                            income:this.income,
                            image:'approved',
                            barangay: this.barangay,
                      
                            email:this.email,
                            status: 'approved',
                        })
                        .then((docRef) => {

                          firebase.firestore().collection('admin').where("barangay","==", this.barangay).get().then(snapshot => {
                              napshot.docs.forEach(docs => {
                                        this.barangay_id = docs.id;
                                        firebase.firestore().collection("admin").doc(this.barangay_id).update({
                                            population: firebase.firestore.FieldValue.increment(1)
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

                         this.members.forEach((docs,index) => {
                           firebase.firestore().collection("members").add({
                            name: this.members[index],
                            address:this.address,
                            age:this.age_household[index],
                            sex:this.sex_household[index],
                            employment:this.employment_household[index],
                            income: "Is only a member" ,
                            image: "Is only a member",
                            barangay: this.barangay,
                            email:this.email,
                            status: 'approved',
                        })
                            firebase.firestore().collection("admin").doc(this.barangay_id).update({
                              population: firebase.firestore.FieldValue.increment(1)
                            }).then(function() {
                            console.log("Document successfully updated!");
                            })
                            .catch(function(error) {
                                console.error("Error updating document: ", error);
                            });
                         })

                            alert("User Registerd");
                            
                            console.log("Document written with ID: ", docRef.id);

                        })
                        .catch(function(error) {
                            console.error("Error adding document: ", error);
                        });
                                            

      
        },

        addList(){

          if(this.age_household_model >= 60)
          {
            this.employment_household_model = "Senior"
          }
          this.members = [...this.members, this.member_model];
          this.age_household = [...this.age_household, this.age_household_model];
          this.employment_household = [...this.employment_household, this.employment_household_model];
          this.sex_household = [...this.sex_household, this.sex_household_model];
          this.member_model = '';
          this.employment_household_model = '';
          this.sex_household_model = '';
          this.age_household_model = '0';
        },

        removeIndex(member, employment_household, sex){
          this.members = this.members.filter(del => del !== member);
          this.employment_household = this.members.filter(del => del !== employment_household);
          this.sex_household = this.members.filter(del => del !== sex);
        }

        
    },

    data() {
    return {
        name:'',
        address:'',
        age:0,
        sex:'',
        email:'',
        employment:'',
        income:'',
        barangay:'',
        barangay_id:'',

        //Household memeber data
        members:[],
        member_model:'',
        employment_household:[],
        employment_household_model:'',
        sex_household:[],
        sex_household_model:'',
        age_household:[],
        age_household_model:0,
    };
    },
}

</script>

<style scope>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: "Nunito", sans-serif;
}

button {
  border: none;
  outline: none;
  width: 100%;
  padding: 15px 0;
  color: #fff;
  font-size: 16px;
  letter-spacing: 1px;
  background: #603bbb;
  cursor: pointer;
}

.login {
  width: 100%;
  min-height: 100vh;
  padding: 0 20px;
  background: #e9e9e9;
  display: flex;

}

.login .loginContainer {
  padding: 30px;
  margin: auto;
  width: 100%;
  max-width: 50rem;
  min-height: 500px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: radial-gradient(
    ellipse at left bottom,
    rgba(22, 24, 47, 1) 0%,
    rgba(38, 20, 72, 0.9) 59%,
    rgba(17, 27, 75, 0.9) 100%
  );
  box-shadow: 0 50px 70px -20px rgba(0, 0, 0, 0.8);
     margin-top: 20px;
}

.login .loginContainer h1{
  color: white;
  margin: 14px 0;
  display: flex;
  justify-content: center;
  font-size: 28px;
  line-height: 1;
}

.login .loginContainer label {
  color: white;
  margin: 14px 0;
  display: block;
  font-size: 18px;
  line-height: 1;
}

.login .loginContainer input {
  width: 100%;
  border: none;
  outline: none;
  font-size: 16px;
  padding: 10px;
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  letter-spacing: 1px;
}

.login .loginContainer input::placeholder {
  
  color: #afafaf;
  letter-spacing: 1px;
}


.login .loginContainer .btnContainer {
  width: 100%;
  padding: 24px 0;
}

.login .loginContainer .btnContainer p {
  margin: 14px 0 0 0;
  text-align: right;
  color: #fff;
}

.login .loginContainer .btnContainer p span {
  color: yellow;
  font-weight: 500;
  letter-spacing: 0.5px;
  margin-left: 5px;
  cursor: pointer;
  transition: all 400ms ease-in-out;
}

.login .loginContainer .btnContainer p span:hover {
  color: red;
}

.login .loginContainer .errorMsg {
  color: red;
  font-size: 16px;
}

.login .loginContainer select{
  width: 100%;
  border: none;
  outline: none;
  font-size: 19px;
  padding: 10px;
  background: rgba(24, 2, 29, 0.4);
  color:#fff;
  letter-spacing: 1px;
}

.login .loginContainer input[type="file"] {
max-width: 100%;
white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.list{
  display: flex;
  flex-direction: row;
}
.list button{
  max-width: 15%;
}

.border p{
  color: yellow;
  font-size: 1rem;
}

/* Tables */

table{
  color:whitesmoke;
}

.hypeLink{
  text-decoration: underline;
  color: white;
}

.hypeLink:hover{
  font-weight: bolder;
  color:whitesmoke;
  cursor: pointer;
}

.border{
  margin: .5rem;
  border-style: dashed;
  border-radius: 5%;
  padding: 1rem;
}


</style>