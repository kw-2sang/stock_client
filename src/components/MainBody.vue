<script setup>
import { ref } from 'vue';
const apiData = ref([]);
fetch("http://api.2sang.site/stock/")
.then(async (response) => {
  const data = await response.json();
  apiData.value = data;
});
</script>
<template>
  <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
      <h1 class="h2">Stock List</h1>
      <!-- <div class="btn-toolbar mb-2 mb-md-0">
        <div class="btn-group me-2">
          <button type="button" class="btn btn-sm btn-outline-secondary">Share</button>
          <button type="button" class="btn btn-sm btn-outline-secondary">Export</button>
        </div>
        <button type="button" class="btn btn-sm btn-outline-secondary dropdown-toggle">
          <span data-feather="calendar" class="align-text-bottom"></span>
          This week
        </button>
      </div> -->
    </div>

    <!-- <canvas class="my-4 w-100" id="myChart" width="900" height="380"></canvas> -->

    <div class="table-responsive">
      <table class="table table-striped table-sm">
        <thead>
          <tr>
            <th scope="col">거래소</th>
            <th scope="col">종목코드</th>
            <th scope="col">종목명</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in apiData">
            <td>KOSPI</td>
            <td><a :href="'chart.html?code=' + item.scode + '&type=year'">{{ item.scode }}</a></td>
            <td>{{ item.sstockname }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
</template>