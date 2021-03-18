<template>
  <div class="container">
    <slot>
      <div id="catfacts">
        <ul v-if="fetchedData">
          <li v-for="item in fetchedData" :key="item.text">
            {{ item.text }}
          </li>
        </ul>
        <div v-if="!fetchedData">loading data...</div>
      </div>
    </slot>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";

@Component({})
export default class Cats extends Vue {
  public fetchedData = null;

  public async fetchData() {
    let aData = await this.$axios.get("https://cat-fact.herokuapp.com/facts");
    let outData = await aData.data;
    this.fetchedData = outData;
  }

  mounted() {
    this.fetchData();
  }
}
</script>