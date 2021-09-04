<template>
    <div class="page-cart">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">
                    Cart
                </h1>
            </div>
            <div class="column is-12 box">
                <table class="table is-fullwidth" v-if="cartTotalLength">
                    <thead>
                        <tr>
                            <th>Product</th>
                            <th>Price</th>
                            <th>Quantity</th>
                            <th>Total</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <CartComponent
                            v-for="item in cart.items"
                            :key="item.product.id"
                            :initialItem="item"
                        />
                    </tbody>
                </table>
                <p v-else>Nothing in your cart..</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import CartComponent from '@/components/CartComponent'
export default {
    name: 'Cart',
    components: {
        CartComponent
    },
    data() {
        return {
            cart: {
                items:[]
            }
        }
    },
    mounted() {
        this.cart = this.$store.state.cart
    },
    computed: {
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        }
    }
}
</script>