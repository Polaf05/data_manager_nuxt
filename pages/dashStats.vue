<template>
    <div>
        <div>
        <label>Representatives per Family</label>
            <column-chart :data="[['Head of the family', head], ['Support', support] , ['Breadwinner', breadwinner], ['Others', others]]"></column-chart>
        </div>

        <div>
        <label>Average Monthly Income</label>
            <pie-chart :data="[['Below 15,000', below15], ['15,000 - 30,000', between1530] , ['30,000 - 50,000', between3050], ['Above 50,000', above50]]"></pie-chart>
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
                         switch(docs.data().role){
                                    case 'Head of the family':
                                        this.head++;
                                    break;
                                    case 'Nurture and Support':
                                        this.support++;
                                    break;
                                    case 'Breadwinner':
                                        this.breadwinner++;
                                    break;
                                    case 'Others':
                                        this.others++;
                                    break;
                            }
                        
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
        breadwinner:0,
        head:0,
        support:0,
        others:0,
        below15:0,
        between1530:0,
        between3050:0,
        above50:0,
        barangay:'',
        user: [],

        };

    },
}

</script>

<style scope>


</style>