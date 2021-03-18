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
import { AxiosResponse } from "axios";
// import Rx from 'rxjs/Rx';
// import VueRx from 'vue-rx';
import { Subject, Subscription } from "rxjs";

const subject = new Subject();

export const messageService = {
  sendMessage: (message: any) => subject.next({ text: message }),
  clearMessages: () => subject.next(),
  getMessage: () => subject.asObservable(),
};

@Component<Cats>({})
export default class Cats extends Vue {
  public fetchedData = [];
  public subscription: Subscription = new Subscription();

  public async fetchData(): Promise<any> {
    let aData = <AxiosResponse>(
      await this.$axios.get("https://cat-fact.herokuapp.com/facts")
    );
    let outData = await aData.data;
    this.fetchedData = outData;
    return outData;
  }

  /*async mounted() {
    this.$subscribeTo(await this.fetchData(), function (val) {
      console.log(val)
    })
    this.fetchData();
  }*/

  created() {
    // subscribe to home component messages
    /*this.subscription = messageService.getMessage().subscribe((fetchData) => {
      if (fetchData) {
        // add message to local state if not empty
        this.fetchedData.push(<never>fetchData);
      } else {
        // clear messages when empty message received
        this.fetchedData = [];
      }
    });*/
    this.fetchData()
  }

  /*beforeDestroy() {
    // unsubscribe to ensure no memory leaks
    this.subscription.unsubscribe();
  }*/
}
</script>