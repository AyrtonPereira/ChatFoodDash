<template>
  <div>
    <Item
      v-for="item in dishList.items"
      :key="item.id"
      :dishName="item.name"
      :dishDescription="item.description"
      :price="item.price"
      :discount="item.discount_rate"
      :imageUrl="item.photo"
    />
  </div>
</template>

<script lang="ts">
/* eslint-disable */
import { Component, Prop, Vue } from "vue-property-decorator";
import axios from "axios";
import Item from "@/components/Menu/Item.vue";

@Component({
  components: {
    Item,
  },
})
export default class Menu extends Vue {
  dishList: object = {}
  
  async getDishes(): Promise<void> {
    const result = await axios.get(
      "https://chatfood-cdn.s3.eu-central-1.amazonaws.com/fe-code-challenge-1/menu.json"
    );
    this.dishList = result.data;
  }

  mounted() {
    this.getDishes()
  }
}

</script>

<style></style>
