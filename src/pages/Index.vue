<template>
  <q-page>
    <q-select
      :options="optCurr"
      label="Currency"
      class="currency"
      v-model="optCurrSelected"
      use-input
      input-debounce="0"
      @filter="filterCurr"
    >
      <template v-slot:no-option>
        <q-item>
          <q-item-section class="text-grey">
            No results
          </q-item-section>
        </q-item>
      </template>
    </q-select>
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

let options = [];

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
      let data = [];
      this.curr.forEach(function(item, index) {
        data.push({
          label: item.bezeich,
          value: item.waehrungsnr
        });
      });
      this.optCurr = data;
      options = data;
    })();
  },
  methods: {
    clearMessage() {
      this.message = "";
    },
    filterCurr(val, update) {
      if (val === "") {
        update(() => {
          this.optCurr = options;
        });
        return;
      }

      update(() => {
        const searchVal = val.toLowerCase();
        this.optCurr = options.filter((item, index) => {
          return item.label.toLowerCase().includes(searchVal);
        });
      });
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
