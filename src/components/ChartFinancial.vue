<script setup>
import { GChart } from 'vue-google-charts'
import { ref } from 'vue'

const queries = window.location.search.split('?')[1].split('&');
const code = queries[0].split('=')[1];
const type = queries[1].split('=')[1];
const year = 2022;
const month = 11;
const day = 1;
// const apiData = ref({});
const chartArray = ref([]);
const load_info = ref(false);
const load_data = ref(false);
const companyInfo = ref([]);
const chartOption = ref({
  hAxis: {
    format: 'yy/M/d'
  },
  candlestick: {
    fallingColor: { strokeWidth: 0, fill: '#a52714' }, // red
    risingColor: { strokeWidth: 0, fill: '#0f9d58' }   // green
  },
  height: 500,
  legend: 'none',
});

const convertTime = (unixTime) => {
  const date = new Date(unixTime * 1)
  return date
}

fetch("http://api.2sang.site/stock/" + code)
.then(async (response) => {
  const data = await response.json();
  return data;
})
.then((data) => {
  companyInfo.value = data[0];
  load_info.value = true;
});

fetch("http://api.2sang.site/stock/data/" + type + '/' + code + '?year=' + year + '&month=' + month + '&day=' + day)
.then(async (response) => {
  const data = await response.json();
  // apiData.value = data;
  return data;
})
.then(async (data) => {
  const keyList = Object.keys(data['Open']);
  chartArray.value.push(['day', 'Low', 'Open', 'Close', 'High']);
  for (let i = 0; i < keyList.length; i++) {
    chartArray.value.push([convertTime(keyList[i]), Object.values(data['Low'])[i], Object.values(data['Open'])[i], Object.values(data['Close'])[i], Object.values(data['High'])[i]]);
  }
})
.then(() => {
  console.log(chartArray.value)
  load_data.value = true;
});
</script>
<template>
  <GChart
    v-if="load_data"
    type="CandlestickChart"
    :data="chartArray"
    :options="chartOption"
  />
  <div class="row mt-3">
    <div class="col-2"></div>
    <div class="col-8">
      <div v-if="load_info" class="mb-3 row">
        <label for="staticEmail" class="col-sm-2 col-form-label">종목코드</label>
        <div class="col-sm-10">
          <input type="text" readonly class="form-control-plaintext" :value="companyInfo['scode']">
        </div>
      </div>
      <div v-if="load_info" class="mb-3 row">
        <label for="staticEmail" class="col-sm-2 col-form-label">회사명</label>
        <div class="col-sm-10">
          <input type="text" readonly class="form-control-plaintext" :value="companyInfo['scompanyname']">
        </div>
      </div>
      <div v-if="load_info" class="mb-3 row">
        <label for="staticEmail" class="col-sm-2 col-form-label">발행 주식 수</label>
        <div class="col-sm-10">
          <input type="text" readonly class="form-control-plaintext" :value="companyInfo['nshares']">
        </div>
      </div>
      <div v-if="load_info" class="mb-3 row">
        <label for="staticEmail" class="col-sm-2 col-form-label">개요</label>
        <div class="col-sm-10">
          <input type="text" readonly class="form-control-plaintext" :value="companyInfo['txoutline']">
        </div>
      </div>
    </div>
    <div class="col-2"></div>
  </div>
</template>