<template>
  <q-page class="flex q-pa-md">
    <!-- flex-center -->
    <div class="row items-center justify-between" style="width: 100%">
      <q-input v-model="cur" label="Курс тенге" />
      <q-btn color="primary" label="с биржи" @click="getCur" />
    </div>
    <div style="width: 100%">
      <div class="column q-pa-md" style="width: 100%">
        <div
          class="row justify-between text-h6"
          v-for="(val, i) in nums"
          :key="i"
        >
          <div>{{ val }}₽</div>
          <div>{{ (val * cur).toFixed(2) }}₸</div>
        </div>
      </div>
      <div class="column q-pa-md" style="width: 100%">
        <div
          class="row justify-between text-h6"
          v-for="(val, i) in nums"
          :key="i"
        >
          <div>{{ val }}₸</div>
          <div>{{ (val / cur).toFixed(2) }}₽</div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",
  data() {
    return {
      cur: 7.3,
      nums: [100, 200, 500, 1000, 5000, 10000],
      kurs: {
        ruble: 0,
        tenge: 0,
      },
    };
  },
  mounted() {
    this.getCur();
  },
  methods: {
    getCur() {
      let thus = this;
      axios.get("https://cdn.cur.su/api/nbkz.json").then((responce) => {
        let data = responce.data;
        let rates = data.rates;
        let rub = rates.RUB;
        let kzt = rates.KZT;
        thus.kurs.ruble = rub;
        thus.kurs.tenge = kzt;

        this.cur = (1 / this.kurs.ruble) * this.kurs.tenge;
      });
    },
  },
});
</script>
