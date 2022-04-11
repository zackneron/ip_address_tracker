<template>
  <div class="result bg-white d-flex">
    <div class="result__item d-flex flex-col">
      <h3 class="result__title text-uppercase">Ip Address</h3>
      <span class="result__description">{{ ipInfo.address }}</span>
    </div>
    <div class="result__item d-flex flex-col">
      <h3 class="result__title text-uppercase">Location</h3>
      <span class="result__description">{{ ipInfo.state }}</span>
    </div>
    <div class="result__item d-flex flex-col">
      <h3 class="result__title text-uppercase">Timezone</h3>
      <span class="result__description">UTC {{ ipInfo.timezone }}</span>
    </div>
    <div class="result__item d-flex flex-col">
      <h3 class="result__title text-uppercase">Isp</h3>
      <span class="result__description">{{ ipInfo.isp }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["ipInfo"],
};
</script>

<style>
.result {
  border-radius: 5px;
  padding: 15px;
  flex-direction: column;
  margin: auto;
  position: absolute;
  bottom: -70px;
  z-index: 20;
  width: 80%;
}

.result__item {
  padding: 10px 0;
}

.result__item .result__title {
  color: var(--dark-gray);
  font-size: 11px;
  margin-bottom: 5px;
  letter-spacing: 1px;
}
.result__item .result__description {
  color: var(--deep-dark-gray);
  font-size: 20px;
  font-weight: 600;
}

@media screen and (min-width: 700px) {
  .result {
    flex-direction: row;
    width: fit-content;
  }
  .result__item {
    padding: 0;
    text-align: left;
    margin: 5px 0 15px 0;
  }
  .result__item:not(:last-child) {
    border-bottom: none;
    border-right: 1px solid var(--dark-gray);
    margin-right: 20px;
    padding-right: 35px;
  }
  .result__item .result__description {
    max-width: 150px;
  }
}
@media screen and (min-width: 975px) {
  .result {
    min-width: 40%;
  }
}
</style>
