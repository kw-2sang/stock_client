<script setup>
const urlSplit = (window.location.href).split('/');
const subUrl = urlSplit[urlSplit.length - 1];
const urlDeleteType = (window.location.href).split('&')[0];
</script>

<template>
  <nav id="sidebarMenu" class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse">
    <div class="position-sticky pt-3 sidebar-sticky">
      <ul class="nav flex-column">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="/">
            <span data-feather="home" class="align-text-bottom"></span>
            Stock List
          </a>
        </li>
        <li v-if="subUrl != ''" class="nav-item">
          <a class="nav-link active" :href="urlDeleteType + '&type=year'">
            <span data-feather="home" class="align-text-bottom"></span>
            연간
          </a>
        </li>
        <li v-if="subUrl != ''" class="nav-item">
          <a class="nav-link active" :href="urlDeleteType + '&type=month'">
            <span data-feather="home" class="align-text-bottom"></span>
            월간
          </a>
        </li>
        <li v-if="subUrl != ''" class="nav-item">
          <a class="nav-link active" :href="urlDeleteType + '&type=day'">
            <span data-feather="home" class="align-text-bottom"></span>
            일간
          </a>
        </li>
      </ul>
    </div>
  </nav>
</template>