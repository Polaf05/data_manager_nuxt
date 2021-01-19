<template>
    <div class="background">
    <div>
        <label> AVERAGE MONTHLY INCOME </label>
            <pie-chart :data="[['Below 15,000', below15], ['15,000 - 30,000', between1530] , ['30,000 - 50,000', between3050], ['Above 50,000', above50]]"></pie-chart>
    </div>

    <div>
        <label> Employment Status </label>
            <column-chart :data="[['Senior', senior], ['Student', student] , ['Employed', employed], ['Unemployed', unemployed]]"></column-chart>
    </div>

    <div>
        <label> Sex Population </label>
            <pie-chart :data="[['Male', male], ['Female', female]]"></pie-chart>
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
        

        firebase.firestore().collection("admin").where("email","==",this.email).get().then((snapshot) => {
            snapshot.docs.forEach((docs) => {
                this.barangay = docs.data().barangay;
                console.log(this.barangay)
              firebase.firestore().collection("user").where("barangay","==",this.barangay).get().then((snapshot) => {
                    snapshot.docs.forEach((docs) => {

                        if(docs.data().status == 'approved'){
                        this.user = [...this.user, docs.data()];
                        
                         switch(docs.data().income){
                                    case 'Below 15,000':
                                        this.below15++;
                                    break;
                                    case '15,000 - 30,000':
                                        this.between1530++;
                                    break;
                                    case '30,000 - 50,000':
                                        this.between3050++;
                                    break;
                                    case '50,000 above':
                                        this.above50++;
                                    break;
                            }
                          switch(docs.data().employment){
                                    case 'Senior':
                                        this.senior++;
                                    break;
                                    case 'Student':
                                        this.student++;
                                    break;
                                    case 'Unemployed':
                                        this.unemployed++;
                                    break;
                                    case 'Employed':
                                        this.employed++;
                                    break;
                            }
                        switch(docs.data().sex){
                                    case 'Male':
                                        this.male++;
                                    break;
                                    case 'Female':
                                        this.female++;
                                    break;
                                    
                            }
                                            
                        }
  
                    });
                });

                firebase.firestore().collection("members").where("barangay","==",this.barangay).get().then((snapshot) => {
                    snapshot.docs.forEach((docs) => {

                        if(docs.data().status == 'approved'){
                        
                          switch(docs.data().employment){
                                    case 'Senior':
                                        this.senior++;
                                    break;
                                    case 'Student':
                                        this.student++;
                                    break;
                                    case 'Unemployed':
                                        this.unemployed++;
                                    break;
                                    case 'Employed':
                                        this.employed++;
                                    break;
                            }

                        switch(docs.data().sex){
                                    case 'Male':
                                        this.male++;
                                    break;
                                    case 'Female':
                                        this.female++;
                                    break;
                                    
                            }
                                            
                        }
  
                    });
                });
          
            });
        });

        
 

    },

    methods:{


    },


    data() {

        return {
        senior:0,
        student:0,
        employed:0,
        unemployed:0,
        below15:0,
        between1530:0,
        between3050:0,
        above50:0,
        barangay:'',
        male:0,
        female:0,
        user: [],

        };

    },
}

</script>

<style scope>

.background{
   background-color: rgb(211,211,211); 
   min-height: 100vh;

}
.background label{
    font-weight: bold;
    font-size: 25px;
    margin-top:10px;
}
</style>