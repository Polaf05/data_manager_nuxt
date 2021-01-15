<template>
    <section class="login">
            <div class="loginContainer">
                <h1>Data Manager</h1> 
                
                <label>Name</label>
                <input type="text" autoFocus required placeholder="Enter Name" v-model="name"/>

                <label>Address</label>
                <input type="text" autoFocus required placeholder="Enter Address" v-model="address"/>

                <label>Age</label>
                <input type="text" autoFocus required placeholder="Enter Age" v-model="age"/>

                <label>Sex</label>
                <select v-model="sex">
                    <option value="Male">Male</option>
                    <option value="Female">Female</option>  
                </select>

                <label>Role</label>
                <select v-model="role">
                    <option value="Head of the family">Head of the family</option>
                    <option value="Nurture and Support">Nurture and Support</option>
                    <option value="Breadwinner">Breadwinner</option>
                    <option value="Others">Others</option>
                </select>

                <label>Monthly Income</label>
                <select v-model="income" place>
                    <option value="Below 15,000">Below 15,000</option>
                    <option value="15,000 - 30,000">15,000 - 30,000</option>
                    <option value="30,000 - 50,000">30,000 - 50,000</option>
                    <option value="50,000 above">50,000 above</option>
                </select>

                <label>Please Submit a valid id</label>
                <input type="file" id="myFile" name="filename" @change="processFile($event)" accept=".jpg,.jpeg" />
                

                <div class="btnContainer">
                   <button @click="uploadFile">Register</button>
                </div>
            </div>

        </section>
</template>

<script>

import firebase from 'firebase/app'
import 'firebase/firestore'
import 'firebase/storage'

export default {
    methods:{

        processFile(event) {
            this.file = event.target.files[0]
            console.log(this.file);
        },

         uploadFile(){
            let file = this.file
                var storageRef = firebase.storage().ref('Valid Id/' + file.name);
                let uploadTask = storageRef.put(file)
                
                uploadTask.on('state_changed', (snapshot) =>{
                }, (error) =>{
                }, () =>{
                    uploadTask.snapshot.ref.getDownloadURL().then((downloadURL) => {
                        console.log('File available at', downloadURL);
                        this.image = downloadURL  

                        firebase.firestore().collection("user").add({
                            name: this.name,
                            address:this.name,
                            age:this.age,
                            sex:this.sex,
                            role:this.role,
                            income:this.income,
                            image:this.image
                            
                        })
                        .then(function(docRef) {
                            console.log("Document written with ID: ", docRef.id);
                        })
                        .catch(function(error) {
                            console.error("Error adding document: ", error);
                        });
                                            
                     });
                });
      
    },

        
    },

    data() {
    return {
        name:'',
        address:'',
        age:'',
        sex:'',
        role:'',
        income:'',
        image:'',

        file: null
      
    };
    },
}

</script>

<style>
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
  padding: 60px;
  margin: auto;
  width: 100%;
  max-width: 520px;
  min-height: 600px;
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
}

.login .loginContainer h1{
  color: white;
  margin: 14px 0;
  display: flex;
  justify-content: center;
  font-size: 33px;
  line-height: 1;
}

.login .loginContainer label {
  color: white;
  margin: 14px 0;
  display: block;
  font-size: 22px;
  line-height: 1;
}

.login .loginContainer input {
  width: 100%;
  border: none;
  outline: none;
  font-size: 19px;
  padding: 10px;
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
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

</style>