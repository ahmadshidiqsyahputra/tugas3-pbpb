<template>
  <ion-page>
    <ion-content>
      <main>
        <h1>Markets</h1>
        <div class="search">&#x2315; Search markets</div>
        <nav>All<b> Crypto</b></nav>
        <p>Sort by Popular</p>
        <section>
          <div class="coin" v-for="c in coins" :key='c.id'>
            <i :style="{background: warna[c.symbol] || '#555' }">{{ c.symbol[0] }}</i>
            <div class="info">
              <b>{{ c.symbol }} <small>/USD</small></b>
              <span>#{{ c.rank }} {{ c.name }}</span>
            </div>
            <div class="harga">
              <b>${{ c.price_usd }}</b>
              <span :class="{ turun: c.percent_change_24h < 0 }">
                {{ c.percent_change_24h >= 0 ?  '\u25b2' : '\u25bc' }}
                {{ Math.abs(c.percent_change_24h) }}%
              </span>
            </div>
          </div>
        </section>
      </main>
    </ion-content>
  </ion-page>
</template>
<script setup>
import{ref, onMounted} from 'vue';
import { IonPage, IonContent } from '@ionic/vue';
import axios from 'axios';
const coins =ref([])
const warna = {
  BTC: '#f7931a', ETH: '#627eea', USDT: '#26a17b',
  BNB: '#f3ba2f', XRP: '#23292f', USDC: '#2775ca',
  SOL: '#9945ff', DOGE: '#c2a633', ADA: '#3468d4'
};

onMounted(async () => {
  const response = await axios.get('https://api.coinlore.net/api/tickers/?limit20');

  coins.value = response.data.data;
});
</script>
<style scoped>
ion-content{--background: #0b0d12 }
main{
  max-width: 550px;
  margin: auto;
  padding: 30px 20px;
  color: white;
}
h1{
  font-size: 40px;
}
.search, section {
  background: #171a21;
  border-radius: 16px;
}
.search{
  padding: 16px;
  color: #888;
  margin: 20px 0;
}
nav {
  margin: 25px;
  color: #888;
}
nav b {
  background: #20252d;
  color: #2dd4bf;
  padding: 12px 20px;
  border-radius: 12px;
}
main > p {
  color: #888;
}
.coin{
  display: flex;
  align-items: center;
  padding: 15px;
  border-bottom: 1px solid #292d35;
}
.coin i{
  width: 45px;
  height: 45px;
  border-radius: 50%;
  display: grid;
  place-items: center;
  margin-right: 12px;
  font-style: normal;
  font-weight: bold;
}
.info{flex: 1;}
.info span{
  display: block;
  color: #888;
  margin-top: 5px;
}
small{
  color: #777;
}
.harga{
  text-align: right;
}
.harga span {
  display: block;
  color: #2dd4bf;
  margin-top: 5px;
}
.harga .turun {
  color: #ef476f;
}
</style>