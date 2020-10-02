<template>
  <q-page>
    <q-select
      :optCurrions="optCurr"
      label="Currency"
      class="currency"
      v-model="optCurrSelected"
    />
    <q-input label="Message" class="message" v-model="message">
      <template v-slot:append>
        <q-btn round dense flat icon="clear" @click="clearMessage" />
      </template>
    </q-input>
    <h5>{{ message }}</h5>
  </q-page>
</template>

<script>
import ky from "ky";
export default {
  data() {
    return {
      message: "I love Vue JS",
      curr: [],
      optCurr: [],
      optCurrSelected: []
    };
  },
  created() {
    (async () => {
      const parsed = await ky
        .post("http://ws1.e1-vhp.com/VHPWebBased/rest/Common/readCurrency", {
          json: {
            request: {
              inputUsername: "sindata",
              inputUserkey: "6D83EFC6F6CA694FFC35FAA7D70AD308FB74A6CD",
              caseType: "8",
              currencyNo: "?",
              currBez: ""
            }
          }
        })
        .json();
      this.curr = parsed.response.tWaehrung["t-waehrung"];
      let test = [];
      this.curr.forEach(function(item, index) {
        test.push({
          label: item.bezeich,
          value: item.waehrungsnr
        });
      });
      this.optCurr = test;
    })();
  },
  methods: {
    clearMessage() {
      this.message = "";
    }
  }
};
</script>

<style lang="stylus" scoped>
.message
  width 300px

.currency
  width 300px
</style>
