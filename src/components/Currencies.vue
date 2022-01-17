<template>
 <div class="container fluid bg-secondary text-white bg-opacity-75">
    <div class="row my-5"> <h3>Currency Exchange Rate Checker App</h3></div>
    <div class="row mb-3"> 
      <div class="col-sm-2 m-1 p-1">
        <input v-model="userInputAmount" placeholder="Summani kiriting...">  
      </div>
      <div class="col-sm-2 m-1 p-1">
        <input v-model="userInput" placeholder="Valyuta kodini yoki davlat nomini kiriting...">  
      </div>
      <div class="col-sm-1 align-self-center"> 

        =>

      </div>
      <div class="col-sm-2 m-1 p-1">
        <input v-model="userInput2" placeholder="Valyuta kodini yoki davlat nomini kiriting...">  
      </div>
      <div class="col-sm-3 m-1 p-1">
        <input v-model="userInputValue" placeholder="Natija"> 
      </div>
      
    </div>
    <div class="row my-2"> </div>
    <div class="row mb-3"> 
      <p>Natija: {{ userInputValueDetailed }}</p>
    </div>
    <button type="button" @click="update" class="btn btn-primary mx-2">Update rates</button>
    <h6 class="my-3">Last updated: {{this.updateDate}}</h6>
    <!-- <div>
        <a v-bind:href="'https://cbu.uz/'" target="_blank">
          <img v-bind:src="'https://cbu.uz/oz/informer/?txtclr=212121&brdclr=FFC700&bgclr=FFE27D&r_choose=USD_EUR_RUB'">
        </a>
    </div> -->
</div>
</template>

<script>
const axios = require('axios');
export default {
  name: 'Currencies',
  data() {
    return {
      userInput:"USD",
      userInput2: "UZS",
      userInputAmount: 1,
      userInputValueTemp:0,
      userInputValue: "",
      userInputValueDetailed: "",
      currencies: [],
      ccy_dto:{
        currencyCode: "",
        nameUz: "",
        nameEn: "",
        nominal: 1,
        rate: 0.0,
        diff: 0.0,
        updatedAt: ""
      },
      updateDate: ""
    }
  },
  watch:{
    userInput: function(ccy){
      this.displayResults(ccy, this.userInput2);
    },
    userInput2:function(ccy){
      this.displayResults(this.userInput, ccy);
    }
  },
  mounted(){
    this.update();
  },
  methods: {
        displayResults: function(ccy, ccy2){
          console.log(ccy + ' ===> ' + ccy2);
          if(ccy != undefined && ccy2 != undefined){
          console.log(ccy + ' ===> ' + ccy2);
          let notFound = true;
            for(let i=0; i < this.currencies.length; i++){
              if(ccy.toLowerCase() == this.currencies[i].currencyCode.toLowerCase() || this.currencies[i].nameUz.toLowerCase().startsWith(ccy.toLowerCase())){
                for(let j=0; j < this.currencies.length; j++){
                  if(ccy2.toLowerCase() == this.currencies[j].currencyCode.toLowerCase() || this.currencies[j].nameUz.toLowerCase().startsWith(ccy2.toLowerCase())){
                    this.userInputValueTemp = this.userInputAmount * this.currencies[i].rate;
                    this.userInputValue = Number(this.userInputValueTemp / (this.userInputAmount * this.currencies[j].rate)).toLocaleString() + " " + this.currencies[j].nameUz;
                    this.userInputValueDetailed =  (Number(this.currencies[i].nominal * this.userInputAmount).toLocaleString() + " " +  this.currencies[i].nameUz + " (" +this.currencies[i].currencyCode+ ") " + " = " + this.userInputValue);
                    //  + " (" + (this.currencies[i].diff > 0 ? ( "+" + this.currencies[i].diff) : this.currencies[i].diff) + ")"
                    notFound = false;
                    break;
                }
                
              }
              if(notFound){
                this.userInputValue = "Davlat nomi yoki valyuta kodiga mos natija topilmadi."
              }
            }
          }
        }
        },
        update: function(){
            axios
            .get(`http://uz-currency-converter-backend.herokuapp.com/updaterates`)
            .then((response) => {
              console.log(response.data);
              this.currencies = response.data;
              this.updateDate = new Date().toString();
              this.displayResults(this.userInput);
            }).catch((error) =>{
              console.log(error);
              console.log('error getting data from API');
            });
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
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
