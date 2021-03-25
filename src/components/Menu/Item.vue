<!--eslint-disable-->
<template>
    <div class="item">
        <div class="text-container">
            <span class="dish-name">{{dishName}}</span>
            <span class="description">{{dishDescription}}</span>
            <div class="price-block">
                <span v-bind:class="[discount ? 'old-price' : 'price']">AED {{parseFloat(price)}}</span>
                <span class="price">{{ discount ? 'AED ' + applyDiscount(price, discount) : ''}}</span>
            </div>
        </div>
        <div class="image-container">
            <img class="item-picture" v-bind:src="imageUrl" />
        </div>
    </div>
</template>

<script lang="ts">
    /* eslint-disable */
    import { Component, Prop, Vue } from "vue-property-decorator";
    import axios from "axios";

    @Component
    export default class Item extends Vue {
        @Prop() private dishName!: string;
        @Prop() private dishDescription!: string;
        @Prop() private price!: string;
        @Prop() private discount!: string;
        @Prop() private imageUrl!: string;

        applyDiscount (price: number, percent: number): number {
            return price - (price * percent);
        }
    }
</script>

<style lang="scss">
    $color-primary: #071C4D;
    $color-secondary: #838DA6;

    .item {
        display: flex;
        flex-direction: row;
        width: 333px;
        align-items: center;
        padding: 21px 0 28px;
        border-bottom: 1px solid #F4F6F9;
        margin: 0 21px;
        
        .text-container {
            justify-content: start;
            display: flex;
            flex-direction: column;
            text-align: start;
            min-width: 235px;

            span { 
                font-family: 'Inter', sans-serif;
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

