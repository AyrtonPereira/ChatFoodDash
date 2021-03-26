<template>
  <div v-if="categories.items.length > 0">
    <span class="category-name">{{categories.name}}</span>
    <Item
      v-for="item in categories.items"
      :key="item.id"
      :dish="item"
      v-on:updateCart="updateCart($event)"
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
    export default class Category extends Vue {
        @Prop() categories!: object;

          updateCart(data: JSON): void {
            this.$emit('updateCart', data);
          } 
    }
</script>

<style>
    .category-name {
        color: #071C4D;
        font-weight: 600;
        font-size: 26px;
        margin: 28px 0 21px;
        display: block;
    }
</style>
