<template>
<div id="vueApp">

    <header class="bg-primary text-white">
      <div class="container text-center">
        <h1>CoinMarketCap</h1>
        <p class="lead">
        <div class="row">
          <div class="col-md-12">
              <input type="text" id="coin" value="" class="form-control" placeholder="ETC">
              <button type="button" name="button" class="btn btn-large"  v-on:click="ara">Ara</button>
          </div>
        </div>
      </p>
      </div>
    </header>
    <section id="about">
      <div class="container">
        <div class="row">
          <div class="col-lg-8 mx-auto">
            <p class="lead"></p>
            <ul>
              <li v-if=" result.status == 'success' "> Sembol : {{ result.symbol }} , Fiyat (USD) : {{ result.price}}  </li>
              <li v-else-if = " result.status == 'error' "> Sonuç Bulunamadı </li>
              <li v-else>  Lütfen bir arama yapınız. </li>
            </ul>
          </div>
        </div>
      </div>
    </section>

  </div>

</template>

<script>
 import axios from 'axios'
 export default{
  el:"#vueApp",
  data(){
  result: '{}'
  },
  created: function(){
  app
  .use(router.routes())
  .use(router.allowedMethods());

   app.listen(3000, ()=>console.log("Started"));
   },
  methods: {
  ara: function() {
              let coin = document.getElementById("coin").value;
              axios.get("http://localhost:3000/coin",
              {
                params : { search : coin },
                headers: {
      	           'Access-Control-Allow-Origin': '*',
      	        }
              }).then(response => this.result = response.data)
              .catch(err => this.result = err.response.data);
            }
          }
        })
  },
  created(){
  router.get('/coin', async ctx => {

let res = await axios({
      method: 'get',
      url: 'https://api.coinmarketcap.com/v1/ticker/',
  }).then(obj => {
      let data = obj.data;
      return data.find( s => s.symbol == ctx.request.query.search);

  }).then(data => {
      if(data)
        return [200, { status : "success", price  : data.price_usd, symbol : data.symbol }];
      else
        return [404,{status : "error" }];
  });

  ctx.status= res[0];
  ctx.body = res[1];

});
 }
</script>
<style>
  .search input{
    padding: 10px 20px;
    border: 1px border #CCC;
    border-radius: 5px;
    width:400px;
  }
  .search button{
    padding: 5px 15px;
    border: 1px solid #CCC;
    color: #FFF;
    font-size: 16px;
    border-radius:5px;
  }
</style>
