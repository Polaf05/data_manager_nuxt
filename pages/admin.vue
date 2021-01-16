<template>
    <div>
    <b-navbar toggleable="lg" type="dark" variant="dark">
        <b-navbar-brand href="/admin">Resident Data Manager</b-navbar-brand>
        
        <b-navbar-nav class="ml-auto">
            <b-button variant="light" @click="signout">Log out</b-button>
        </b-navbar-nav>
    </b-navbar>
    
    <div class="sidebar">
    <a href="#db" @click="tab = 0">Dashboard</a>
    <a href="#tb" @click="tab = 1">List of residents</a>
    <a href="#rev" @click="tab = 2">Review Resident</a>
    <a href="#add" @click="tab = 3">Add Resident</a>
    </div>
    <div class="content">
      <Dashb v-if="tab == 0" v-bind:email = "email" />
      <TableList v-if="tab == 1" v-bind:email = "email" />
      <Review v-if="tab == 2" v-bind:email = "email" />
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

export default {
  components:{
    TableList,
    Review,
    Dashb
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
    firebase.auth().signInWithEmailAndPassword('anonymous@upload.com', 'admin123').then((user) => {
              this.$router.push('/');
            })
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
  background-color: #f1f1f1;
  position: fixed;
  height: 100%;
  overflow: auto;
}

/* Sidebar links */
.sidebar a {
  display: block;
  color: black;
  padding: 16px;
  text-decoration: none;
}

/* Active/current link */
.sidebar a.active {
  background-color: #4CAF50;
  color: white;
}

/* Links on mouse-over */
.sidebar a:hover:not(.active) {
  background-color: #555;
  color: white;
}

/* Page content. The value of the margin-left property should match the value of the sidebar's width property */
div.content {
  margin-left: 20%;
  width: 80%;
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