<template>
    <div>
        
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

        chartData:
        {
            '2017-05-13':2,
            '2018-05-13':5,
            '2019-05-13':7,
        }

        };

    },
}

</script>

<style scope>


</style>