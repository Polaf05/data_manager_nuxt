<template>
    <div class="background">
    <b-navbar sticky toggleable="lg" type="dark" variant="dark">
        <b-navbar-brand href="/admin">Resident Data Manager</b-navbar-brand>
        
        <b-navbar-nav class="ml-auto nav">
            <b-button  @click="signout">Log out</b-button>
        </b-navbar-nav>
    </b-navbar>
    
    <div class="sidebar">
    <a href="#db" @click="tab = 1">Dashboard</a>
    <a href="#tb" @click="tab = 2">List of residents</a>
    <a href="#rev" @click="tab = 3">Review Resident</a>
    <a href="#add" @click="tab = 4">Add Resident</a>
    </div>
    <div class="content">
      
      <div class="welcome" v-if="tab == 0">
        <h1>WELCOME ADMIN!!</h1>
        <div class="welc"> 
          <p> Manage your Residents Data </p>
          <ul>
            <li>View Dashboard </li>
            <li>Access List of Residents</li>
            <li> Review Residents</li>
            <li> Add or Regster Residents Info </li>
          </ul>
          </div>
      </div>
      <Dashb class="main-content" v-if="tab == 1" v-bind:email = "email" />
      <TableList class="main-content"v-if="tab == 2" v-bind:email = "email" />
      <Review class="main-content" v-if="tab == 3" v-bind:email = "email" />
      <Add  class="main-content" v-if="tab == 4" v-bind:admin = "email"/>
    </div>

</div>
</template>

<script>
import firebase from 'firebase/app'
import "firebase/auth"
import "firebase/firestore"
import TableList from './table'
import Review from './review'
import Dashb from './dashStats'
import Add from './addsignup'

export default {
  components:{
    TableList,
    Review,
    Dashb,
    Add
  },
  mounted(){
    firebase.auth().onAuthStateChanged((user) => {
        if (user) {
          this.email = user.email;
        }
      });
  },

  methods:{
    signout(){
    firebase.auth().signOut().then(() => {
              this.$router.push('/');
            }).catch((error) => {
              // An error happened.
        });
    }
  },

  data() {

        return {
          tab: 0,
          email:'',
        };

    },
    
}
</script>

<style scope>

/* The side navigation menu */
.sidebar {
  margin: 0;
  padding: 0;
  width: 20%;
  position: fixed;
  height: 100%;
  overflow: auto;
   background-color: #f1f1f1;
}

/* Sidebar links */
.sidebar a {
  display: block;
  color: black;
  padding: 16px;
  text-decoration: none;
  margin-left: 10px;
  margin-right: 10px;
}

/* Active/current link */
.sidebar a.active {
  background-color: #35495e;
  color: white;
}

/* Links on mouse-over */
.sidebar a:hover:not(.active) {
  background-color: #555;
  color: white;
}

/* Page content. The value of the margin-left property should match the value of the sidebar's width property */
div.content {

  margin-top: 0px;
  margin-left:20%;
  margin-right:0%;
  width: 80%;

}
.welcome{
  margin-left: 0%;
  width: 100%;
  height: 100vh;
  background-image: url("../assets/images/Welcome-BG.jpg");
  padding-top: 200px;
}

.welcome h1{
  background-color: rgb(220,220,220,0.9);
  padding-left: 30%;
 font-weight: bold;
}
.welcome p{
   margin-left: 30%;
   margin-top: 20px;
   font-size: 28px;
    font-weight: bold;
}
.welcome ul{
  font-weight: bold;
  margin-left: 35%;
  list-style: none;
  font-size:20px;
}
.welc{
 
  background-color: 	rgb(169,169,169,0.5);
  
}
.main-content{
  margin-left: 20px;
  margin-top: 20px;
  margin-right: 20px;
}

.nav button{
  background-color: #35495e;
   color: white;
}
/* On screens that are less than 700px wide, make the sidebar into a topbar */
@media screen and (max-width: 700px) {
  .sidebar {
    width: 100%;
    height: auto;
    position: relative;
  }
  .sidebar a {float: left;}
  div.content {margin-left: 0;}
}

/* On screens that are less than 400px, display the bar vertically, instead of horizontally */
@media screen and (max-width: 400px) {
  .sidebar a {
    text-align: center;
    float: none;
   
  }
}


</style>