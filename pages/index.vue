<template>
  <div>
  <b-navbar sticky toggleable="lg" type="dark" variant="dark">
    <b-navbar-brand href="#">Resident Data Manager</b-navbar-brand>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto nav">
            <span> Public Official? </span>
            <nuxt-link to="/login" class="m-1 "><b-button variant="light">Log In</b-button></nuxt-link>
      </b-navbar-nav>
  </b-navbar>
   <b-container fluid class = "background"> 
    <b-row class="landing-container"> 
      <b-col cols = "6" class="about">
             <h1> MANAGE RESIDENTS DATA </h1> 
              <span> EASILY /  EFFICIENTLY / HUSTLE FREE </span> 
              <br/><br/> 
      
              <p> We are making your data collection as easy as possible.</p>
              <ul>
                <li> Collect Residents Data Online</li> <br/>
                <li> Filter out Infromation that you need </li>  <br/>
                <li> Preview Statistics at ease </li>
              </ul>
      </b-col>      
  
      <b-col cols = "4" >
        <b-row>
          <b-col cols = "12" class="reg-container "> 
            <h1> ONLINE REGISTRATION </h1>
            <span> Resident of Manila? </span>
            <br/><span> Register your information online to your Barangay </span> <br/> <br/>
            <b-button class="button-style" variant="light" @click="signin">Register</b-button>
          </b-col>
        </b-row>
      </b-col>
    </b-row>
   </b-container> 
    
</div>
</template>

<script>
import firebase from 'firebase/app'
import 'firebase/auth'
import 'firebase/firestore'
import 'firebase/storage'

export default {

  methods: {
    signin(){
    firebase.auth().signInWithEmailAndPassword('anonymous@upload.com', 'admin123').then((user) => {
              this.$router.push('/signup');
            })
    }
  },

}
</script>

<style lang="css" scoped>

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
   background-color:rgba(17, 27, 75, 0.9) ;

}
.nav span{
  color: white;
  margin-top: 10px;
  margin-right: 5px;
}
.nav button{
 background-color:rgba(17, 27, 75, 0.9) ;
 color: white;
}
.button-style{
  background-color:rgba(17, 27, 75, 0.9) ;
  width: 200px;
  height: 50px;
  color: white;
  margin-left: 110px;

  
}

.reg-container{
  margin-top: 150px;
  color: white;
  font-size: 15px;
  background-color:  rgba(169,169,169, 0.1);
  height: 260px;
  padding: 15px;
  max-width: 500px;
}
.reg-container h1{
  margin-top: 45px;
  font-size: 28px;
  text-align: center;
  font-weight: bold;

}
.reg-container span{
  margin-left: 43px;
  font-size: 15px;
  
}
.about{
  color: white;
  justify-content: center;
  align-items: center;
  margin-top: 50px;
  margin-left: 100px;
}
.about h1{
  font-size: 40px;
  font-weight: bold;
}
.about span{
  font-size: 23px;
  margin-left: 50px;
}
.about p{
  font-size: 22px ;
}
.about ul{
  list-style: none;
  margin-left: 20px;
  font-size: 20px;
}
.landing-container{
  padding: 0px;
  min-height: 100vh;

 
}
.background{
 background-image: url("../assets/images/Manila-City-Hall-Night.jpg");
 background-size: 100% 100% ;
 background-repeat: no-repeat;
 background-blend-mode: darken;
 /*background-color: rgba(17, 27, 75, 0.8); */

}

</style>
