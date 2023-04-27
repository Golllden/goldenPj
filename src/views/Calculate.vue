<template>
  <div class="calculate-page">

            <!-- 金飾 -->
            <div class="section">
                <h2 class="colculate-title">飾金計算</h2>
                <h3 class="public-price-title">當日公告回收價</h3>
                <div class="public-price">
                    <div class="unit-box penny">
                        錢 : {{ goldPennyPrice }}
                    </div>
                    <div class="unit-box gram">
                        克 : {{ goldGramPrice }}
                    </div>
                </div>
                <div class="cal-section">
                    <!-- left -->
                    <div class="cal-left">
                        <div>
                            單位
                            <select v-model="goldUint">
                                <option value="penny">錢</option>
                                <option value="gram">克</option>
                            </select>
                        </div>
                        <div class="input-section">
                            <input 
                                class="input-style" 
                                type="number" 
                                placeholder="輸入重量"
                                v-model="goldWeight"
                            >
                            <button
                                class="sumbit-btn"
                                @click="calPriceGold"
                            >
                            計算
                            </button>
                        </div>
                    </div>
                    <!-- right -->
                    <div class="cal-result">
                        <div>
                            <div>可兌現金額</div>
                            <div class="cash-price"> {{ goldCalPrice }}</div>
                        </div>
                        <div>
                            <div>可兌USDT(匯率:<strong class="uRate">{{ usdtRate }}</strong>)</div>
                            <div class="cash-price"> {{ gold_calculatedUsdt }} USDT</div>
                        </div>
                    </div>
                </div>
                <p>例重：一兩二錢三分四厘=12.34</p>
                <hr>
            </div>

            <!-- 金條 -->
            <div class="section">
                <h2 class="colculate-title">黃金條塊計算</h2>
                <h3 class="public-price-title">當日公告回收價</h3>
                <div class="public-price">
                    <div class="unit-box penny">
                        錢 : {{ bullionPennyPrice }}
                    </div>
                    <div class="unit-box gram">
                        克 : {{ bullionGramPrice }}
                    </div>
                </div>
                <div class="cal-section">
                    <!-- left -->
                    <div class="cal-left">
                        <div>
                            單位
                            <select v-model="bullionUint">
                                <option value="penny">錢</option>
                                <option value="gram">克</option>
                            </select>
                        </div>
                        <div class="input-section">
                            <!-- 條塊重量 -->
                            <input 
                                class="input-style" 
                                type="number" 
                                placeholder="輸入重量"
                                v-model="billionWeight"
                            >
                            <button
                                class="sumbit-btn"
                                @click="calPriceBullion"
                            >
                            計算
                        </button>
                        </div>
                    </div>
                    <!-- right -->
                    <div class="cal-result">
                        <div>
                            <div>可兌現金額</div>
                            <div class="cash-price"> {{ bullionCalPrice }}</div>
                        </div>
                        <div>
                            <div>可兌USDT(匯率:<strong class="uRate">{{ usdtRate }}</strong>)</div>
                            <div class="cash-price"> {{ bullionCalculatedUsdt }} USDT</div>
                        </div>
                    </div>
                </div>
                <p>例重：一兩二錢三分四厘=12.34</p>
                <hr>
            </div>
  </div>
</template>

<script>

// import axios套件
import axios from 'axios';


export default {
  name: 'calculatePage',
  data () {
    return{
      usdtRate:0, //usdt匯率 from Api
      bullionPennyPrice:0, //一錢價格 by 金條 from Api


    //  金飾
      goldCalPrice: 0, //計算金飾
      goldUint:'penny', //飾金單位
      goldWeight:0, //金飾重量
      goldCalculatedUsdt:0,
//
    //  金條 
      bullionCalPrice: 0, //計算金條價格
      bullionUint:'penny', //金條單位
      billionWeight:0,
      bullionCalculatedUsdt:0,
    }
  },
  
  computed:{
        // 一克價格(金條)
        bullionGramPrice: function(){
            return (this.bullionPennyPrice/ 3.75).toFixed(2)
        },

        gold_calculatedUsdt: function(){
            return (this.goldCalPrice/this.usdtRate).toFixed(2)
        },

        // 一錢價格(金飾)
        goldPennyPrice: function(){
           return (this.bullionPennyPrice-200)* 0.95;
        },
        // 一克價格(金飾)
        goldGramPrice: function(){
            return (this.goldPennyPrice/ 3.75).toFixed(2);
        }
    },

    methods:{
        // 條塊計算
        calPriceBullion: function(){

            let priceBase = this.bullionUint == 'penny'? this.bullionPennyPrice:this.bullionGramPrice;
            // 計算結果

            // 總價
            let calculatedResult = this.billionWeight * priceBase;
            this.bullionCalPrice = calculatedResult

            // usdt
            this.bullionCalculatedUsdt = (calculatedResult/ this.usdtRate).toFixed(2)

            alert("試算完成")
        },

        // 金飾計算
        calPriceGold: function(){


            let priceBase = this.goldUint == 'penny'? this.goldPennyPrice:this.goldGramPrice;
            // 計算結果

            // 總價
            let calculatedResult = this.goldWeight * priceBase;
            this.goldCalPrice = calculatedResult

            // usdt
            this.goldCalculatedUsdt = (calculatedResult/ this.usdtRate).toFixed(2)

            alert("試算完成")
            },

        // 取得USDT匯率
        getUsdtRate: function(){

            let UsdApi = "http://13.212.61.53:3000/api/price/usprice";

            axios.get(UsdApi)
            .then( (response) => this.usdtRate = response.data.usprice[0].usprice)
            .catch( (error) => console.log(error))

        },

        // 取得黃金一錢多少
        getGoldPrice: function(){

            let golaAip = "http://13.212.61.53:3000/api/price/goldprice"

            axios.get(golaAip)
            .then( (response) => this.bullionPennyPrice = response.data.goldprice[0].goldprice)
            .catch( (error) => console.log(error))
        },

        setNewPrice: function(){

            let revisePriceApi = "http://13.212.61.53:3000/api/price/priceset";

            axios.post(revisePriceApi,{
                usprice: 31.05,
                goldprice: 7250
            })
                .then( (response) => console.log(response))
                .catch( (error) => console.log(error))

        },

    },

    created: function(){
        this.getUsdtRate();
        this.getGoldPrice();
    },

}
</script>


<style lang="scss" scoped>


.section{
    color: #666666;
    margin-bottom: 50px;
}

.colculate-title{
    padding:5px;
    background-color: #ead4a6;
}

.public-price-title{
    margin-top: 20px;
    text-align: center;
    padding: 3px;
}

.public-price{
    display: flex;
}

.unit-box{
    flex: 1;
    padding: 3px;
    border: 1px solid #121212;
    border-radius: 2px;
    text-align: center;
}

.cal-section{
    display: flex;
    padding: 15px;
}

.cal-left, .cal-result{
    width: 50%;
}

.cal-result{
    text-align: center;
    font-size: 1rem;
}

.cash-price{
    font-size: 2rem;
    color: red;
}


.input-section{
    margin-top: 20px;
}

.input-style{
    border-radius: 5px;
    width: 120px;
    height: 25px;
}

.uRate{
    font-size: 1.25rem;
    color: rgb(55, 50, 126)
}


.sumbit-btn{
    padding: 3px;
    border: none;
    border-radius: 5px;
    background-color: #303030;
    color: white;
    cursor: pointer;
}

@media screen and (max-width: 480px) {

}

</style>