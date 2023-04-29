<template>
  <div class="modal-box">
      <strong>修改
        <span 
          class="close-btn"
          @click="closeModal"
        >
          x
        </span>
      </strong>

      <input v-model="newGoldPrice" type="number" placeholder="金價">
      <input v-model="newUsdtRate" type="number" placeholder="USDT匯率">
      <input v-model="ping" type="text" placeholder="密">
      <button @click="submitRevise">送出</button>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'modalPop',
  data(){
    return{
      newGoldPrice:'',
      newUsdtRate:'',
      ping:'',
    }
  },
  methods:{

    submitRevise: function(){

      if(this.newGoldPrice &&this.newUsdtRate&& this.ping == '0897'){
        this.setNewPrice();
        return
      }

      alert('操作有誤');

    },

    setNewPrice: function(){

      let revisePriceApi = "http://13.212.61.53:3000/api/price/priceset";

      axios.post(revisePriceApi,{
          usprice: this.newUsdtRate,
          goldprice: this.newGoldPrice,
      })
          .then( (response) => {
            console.log(response);
            alert('OK');
          })
          .catch( (error) => {
            console.log(error);
          })

      },

      closeModal: function(){
        this.$emit('closeEvt');
      }

  }
  
}
</script>

<style scoped lang="scss">


.modal-box{
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 300px;
  height: 180px;
  border-radius: 5px;
  background-color: #e58f8f;
  position: relative;
  left:50%;
  transform: translateX(-50%);
}

strong{
  padding: 10px 0;
}

input{
  width: 70%;
  margin: 3px 0;
  padding: 3px 0;
}

button{
  padding: 5px;
  margin-top: 8px;
  color: white;
  border: none;
  border-radius: 3px;
  background-color: #5b695e;
  cursor:pointer
  }
  
  .close-btn{
    padding: 2px;
    border-radius: 3px;
    border:1px solid white;
    position: relative;
    left: 210%;
    cursor: pointer;
  }

</style>