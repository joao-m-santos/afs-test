<template>
  <div class="card transfer-row">
    <div class="card-content">
      <span class="box tag is-medium" v-bind:class="[classObject]">{{
        transfer.state
      }}</span>
      <h4>{{ transfer.fromSecurityHolder }}</h4>
      <p>Type: {{ transfer.type }}</p>
      <p>Amount: {{ transfer.amount }}</p>
      <strong v-if="transfer.forgottenProperty">{{
        transfer.forgottenProperty
      }}</strong>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
import { Transaction } from "@/types/types";

@Component({
  name: "TransferRow"
})
export default class TransferRow extends Vue {
  @Prop({ required: true }) transfer!: Transaction;

  // Since the project already imports Bulma.css, I used some of its classes

  get classObject(): string {
    switch (this.transfer.state) {
      case "NEW":
        return "is-info";
      case "MODIFIED":
        return "is-warning";
      case "PUBLISHED":
        return "is-success";
      case "OLD":
      default:
        return "";
    }
  }
}
</script>

<style lang="scss">
.transfer-row {
  margin: 1.5rem;
  max-width: 500px;
  text-align: left;

  > .card-content {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  /* To create look similar to provided UI designs */
  .tag {
    position: absolute;
    top: 1rem;
    right: -1rem;
  }
}
</style>
