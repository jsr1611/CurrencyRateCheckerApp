<template>
 <div class="container fluid bg-secondary text-white bg-opacity-75">
    <div class="row my-5"> <h3>Currency Exchange Rate Checker App</h3></div>
    <div class="row mb-3"> 
      <input v-model="userInput" placeholder="Valyuta kodini yoki davlat nomini kiriting...">
    </div>
    <div class="row mb-3"> 
      <p>Natija: {{ userInputValue }}</p>
    </div>
    <button type="button" @click="update" class="btn btn-primary mx-2">Update rates</button>
    <h6 class="my-3">Last updated: {{this.updateDate}}</h6>
</div>
</template>

<script>
const axios = require('axios');
export default {
  name: 'Currencies',
  data() {
    return {
      userInput:"",
      userInputValue:"",
      currencies: [],
      ccy_dto:{
        currencyCode: "",
        nameUz: "",
        nameEn: "",
        nominal: 1,
        rate: 0.0,
        diff:0.0,
        updatedAt:""
      },
      updateDate: ''
    }
  },
  watch:{
    userInput: function(ccy){
      let notFound = true;
      for(let i=0; i < this.currencies.length; i++){
        if(ccy == this.currencies[i].currencyCode || this.currencies[i].nameUz.startsWith(ccy)){
          this.userInputValue = (this.currencies[i].nominal + " " +  this.currencies[i].nameUz + " (" +this.currencies[i].currencyCode+ ") " + " = " + this.currencies[i].rate + " so`m (" + (this.currencies[i].diff > 0 ? ( "+" + this.currencies[i].diff) : this.currencies[i].diff) + ")");
          notFound = false;
          break;
        }
      }
      if(notFound){
          this.userInputValue = "Davlat nomi yoki valyuta kodiga mos natija topilmadi."
      }
      
    }
  },
  mounted(){
    this.update();
  },
  methods: {
        update: function(){
            axios
            .get(`http://uz-currency-converter-backend.herokuapp.com/updaterates`)
            .then((response) => {
              console.log(response.data);
              this.currencies = response.data;
              this.updateDate = new Date().toString();
            }).catch((error) =>{
              console.log(error);
            });
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
