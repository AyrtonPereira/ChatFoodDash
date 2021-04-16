<template>
  <div class="category-box" v-if="categories.items.length > 0">
    <h2 class="category-name">{{ categories.name }}</h2>
    <Item
      v-for="item in categories.items"
      :key="item.id"
      :dish="item"
      v-on:updateCart="updateCart($event)"
    />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import Item from "@/components/Menu/Item.vue";

@Component({
  components: {
    Item
  }
})
export default class Category extends Vue {
  @Prop() categories!: object;

  updateCart(data: JSON): void {
    this.$emit("updateCart", data);
  }
}
</script>

<style lang="scss">
.category-box {
  text-align: left;

  .category-name {
    color: #071c4d;
    font-weight: 600;
    font-size: 26px;
    margin: 28px 21px 21px;
    display: block;
  }
}
</style>
