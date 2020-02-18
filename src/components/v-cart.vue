<template>
    <div class="v-cart">
        <router-link :to="{name: 'catalog'}">
            <div class="v-catalog__link_to_cart">Back to catalog</div>
        </router-link>
        <h1>Cart</h1>
        <p v-if="!cart_data.length">There are no products in cart</p>
        <v-cart-item
                v-for="(item, index) in cart_data"
                :key="item.article"
                :cart_item_data="item"
                @deleteFromCart="deleteFromCart(index)"
                @decrement="decrement(index)"
                @increment="increment(index)"
        />
        <div class="v-cart__total">
            <p>Total: </p>
            <p>{{cartTotalCost}}</p>
        </div>
    </div>
</template>

<script>
    import vCartItem from './v-cart-item'
    import {mapActions} from 'vuex'

    export default {
        name: "v-cart",
        props: {
          cart_data: {
              type: Array,
              default() {
                  return [];
              }
          }
        },
        components: {
            vCartItem
        },
        methods: {
            ...mapActions([
                'DELETE_FROM_CART',
                'INCREMENT_CART_ITEM',
                'DECREMENT_CART_ITEM'
            ]),
            deleteFromCart(index) {
                this.DELETE_FROM_CART(index);
            },
            increment(index) {
                this.INCREMENT_CART_ITEM(index);
            },
            decrement(index) {
                this.DECREMENT_CART_ITEM(index);
            }
        },
        computed: {
            cartTotalCost() {
                let result = [];

                if (this.cart_data.length) {
                    for (let item of this.cart_data){
                        result.push(item.price * item.counter);
                    }

                    result = result.reduce(function (sum, element) {
                        return sum + element;
                    });
                    return result;
                } else return 0;

            }
        }
    }
</script>

<style lang="less">
    .v-cart {
        margin-bottom: 100px;
        &__total {
            position: fixed;
            bottom: 0;
            right: 0;
            left: 0;
            padding: 24px;
            display: flex;
            justify-content: center;
            background-color: #0e0b0e;
            color: #fff;
            font-size: 20px;
        }
    }
</style>