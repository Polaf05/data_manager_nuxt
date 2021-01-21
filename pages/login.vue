<template>
     <section class="login">
            <div class="loginContainer">

                <h1>Data Manager</h1> 
              <div v-if="!adminPrive">  
                <label v-if="flag">Barangay</label>
                <select v-model="barangay" v-if="flag">
                    <option value="brgy 101">brgy 101</option>
                    <option value="brgy 102">brgy 102</option>
                    <option value="brgy 103">brgy 103</option>
                    <option value="brgy 104">brgy 104</option>
                    <option value="brgy 105">brgy 105</option>
                </select>

                <label>Email</label>
                <input type="text" autoFocus required placeholder="Enter Email Address" v-model="email"/>

                <label>Password</label>
                <input type="password" autoFocus required placeholder="Enter Password" v-model="password"/>

                <div class="btnContainer">
                   <button v-if="!flag" @click="login">Log In</button>
                    <button v-if="flag" @click="signup">Register</button>

                </div>
              </div>
                <div v-if="codeFlag">
                  <div>
                      <input type="password" autoFocus required placeholder="Enter Admin Code" style="text-align:center" v-model="code"/>
                      <button style="margin-top: 1rem" @click="submitCode">Submit</button>
                  </div>
                </div>

                <div class="btnContainer">
                  <p v-if="!adminPrive">
                        Don't have an account?
                              <span v-on:click="adminPrive = true, codeFlag = true" >Sign Up </span>
                  </p>

                  <p v-if="adminPrive">
                        Have an account?
                              <span v-on:click="adminPrive = false,  codeFlag = false, flag = false"  >Sign In </span>
                  </p>
                </div>
            </div>
        </section>
</template>

<script>
import firebase from 'firebase/app'
import "firebase/auth"
import "firebase/firestore"

export default {
    methods:{
    state() {
      firebase.auth().onAuthStateChanged((user) => {
        if (user) {
          console.log(user);
        }
      });
    },

    submitCode(){
      if(this.code == "chulsan123"){
        this.flag=true;
        this.adminPrive=false;
        this.code = '';
        this.codeFlag=false;
      }
    },

    signup() {
      firebase.auth().createUserWithEmailAndPassword(this.email, this.password).then((user) => 
      {
          firebase.firestore().collection("admin").add({
                barangay: this.barangay,
                email: this.email,
                
            })
            .then(function(docRef) {
                console.log("Document written with ID: ", docRef.id);
            })
            .catch(function(error) {
                console.error("Error adding document: ", error);
            });

          alert("Thank you for signing up");
          console.log(user);
          firebase
            .auth()
            .currentUser.sendEmailVerification()
            .then((user) => {
              alert("Verification Email Sent");
              this.$router.push('/admin');
            })
            .catch((error) => {
              console.log(error);
              this.error = error;
            });
        }).catch(function(error) {
          var errorCode = error.code;
          var errorMessage = error.message;
          if (errorCode == "auth/weak-password") {
            alert("The password is too weak.");
          } else {
            alert(errorMessage);
          }
          console.log(error);
        });
    },

    login() {
            firebase.auth().signInWithEmailAndPassword(this.email, this.password).then((user) => {

              this.$router.push('/admin');

            }).catch(function(error) {
                // Handle Errors here.
                var errorCode = error.code;
                // [START_EXCLUDE]
                if (errorCode === "auth/wrong-password") {
                    alert("Wrong password.");
                }
                if (errorCode === "auth/invalid-email") {
                    alert("Invalid email.");
                }
                if (errorCode === "auth/user-disabled") {
                    alert("User disabled.");
                }
                if (errorCode === "auth/user-not-found") {
                    alert("User Not Found");
                }
                console.log(error);
                // document.getElementById('login').disabled = false;
                // [END_EXCLUDE]
            });

        },

    },
    data: () => {
    return {
      email: "",
      password: "",
      flag:false,
      barangay:'',
      adminPrive:false,
      code:'',
      codeFlag:false,
      
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
  background: #e9e9e9;
  display: flex;
}

.login .loginContainer {
  padding: 30px;
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
  margin-top: 50px;
}

.login .loginContainer h1{
  color: white;
  margin: 14px 0;
  display: flex;
  justify-content: center;
  font-size: 33px;
  line-height: 1;
  margin-top: 0px;
}

.login .loginContainer div label {
  color: white;
  margin: 14px 0;
  display: block;
  font-size: 20px;
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
.btnContainer p{
 
  text-align: center;
}

.login .loginContainer .btnContainer p {
  font-weight: 500;
  margin-left: 5px;
  cursor: pointer;
  text-align: center;
}
.login .loginContainer .btnContainer span {
  color: yellow;
  font-weight: 500;
  margin-left: 5px;
  cursor: pointer;
  justify-content: center;
  align-items: center;
}

.login .loginContainer .btnContainer p span:hover {
  color: red;
}

.login .loginContainer .errorMsg {
  color: red;
  font-size: 16px;
}
.login .loginContainer div select{
  width: 100%;
  border: none;
  outline: none;
  font-size: 19px;
  padding: 10px;
  background: rgba(24, 2, 29, 0.4);
  color:#fff;
  letter-spacing: 1px;
}

</style>