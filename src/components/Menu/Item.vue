<template>
  <div
    v-if="dish.stock.availability > 0 || dish.selected > 0"
    class="item"
    v-bind:class="{ selected: dish.selected > 0 }"
    v-on:click="
      $emit('updateCart', { id: dish.id, category: dish.category_id })
    "
  >
    <div class="text-container">
      <span class="dish-name"
        ><span v-if="dish.selected > 1">{{ dish.selected + " x " }}</span
        >{{ dish.name }}</span
      >
      <span class="description" :title="dish.description">{{
        dish.description
      }}</span>
      <div class="price-block">
        <span v-bind:class="[dish.discount_rate ? 'old-price' : 'price']"
          >AED {{ parseFloat(dish.price) }}</span
        >
        <span class="price">{{
          dish.discount_rate
            ? "AED " + applyDiscount(dish.price, dish.discount_rate)
            : ""
        }}</span>
      </div>
    </div>
    <div class="image-container">
      <img class="item-picture" v-bind:src="dish.photo" />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Item extends Vue {
  @Prop() private dish!: object;

  applyDiscount(price: number, percent: number): number {
    return price - price * percent;
  }

  showMessage(name: string): void {
    alert(name);
  }
}
</script>

<style lang="scss">
$color-primary: #071c4d;
$color-secondary: #838da6;

.item {
  display: flex;
  flex-direction: row;
  width: 333px;
  align-items: center;
  padding: 21px 0 28px;
  border-bottom: 1px solid #f4f6f9;
  margin: 0 21px;
  cursor: pointer;

  &.selected {
    margin-left: 0;

    .text-container {
      border-left: 7px solid #1258ff;
      padding-left: 14px;
    }
  }

  .text-container {
    justify-content: start;
    display: flex;
    flex-direction: column;
    text-align: start;
    min-width: 235px;
    padding-right: 7px;

    span {
      font-family: "Inter", sans-serif;
      line-height: 21px;
    }
    .dish-name {
      color: $color-primary;
      font-weight: 600;
      font-size: 16px;
      width: 100%;
      margin-bottom: 7px;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }
    .description {
      font-size: 14px;
      color: $color-secondary;
      width: 100%;
      margin-bottom: 7px;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      max-height: 42px;
      overflow: hidden;
    }
    .price-block {
      flex-direction: row;
      display: flex;

      .price {
        font-weight: 600;
        font-size: 14px;
        color: $color-primary;
        order: 1;
        margin-right: 14px;
      }
      .old-price {
        color: $color-secondary;
        font-size: 14px;
        text-decoration-line: line-through;
        order: 2;
      }
    }
  }

  .image-container {
    border-radius: 7px;

    .item-picture {
      max-width: 91px;
      object-fit: contain;
      border-radius: 7px;
    }
  }
}
</style>