<template>
    <tr>
        <td><router-link :to="item.product.get_absolute_url">{{ item.product.name }}</router-link></td>
        <td>${{ item.product.price }}</td>
        <td>
            <a @click="decreaseQuantity(item)">-</a>
            {{ item.quantity }}
            <a @click="increaseQuantity(item)">+</a>
            
        </td>
        <td>${{ getItemTotal(item).toFixed(2) }}</td>
        <td><button class="delete" @click="removeFromCart(item)"></button></td>
    </tr>
</template>

<script>
export default {
    name: 'CartComponent',
    props:{
        initialItem: Object
    },
    data(){
        return {
            item: this.initialItem
        }
    },
    methods:{
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        increaseQuantity(item) {
            item.quantity += 1
            this.updateCart()
        },
        decreaseQuantity(item) {
            
            item.quantity -= 1
            if(item.quantity == 0){
                this.$emit('removeFromCart', item)    
            }
            this.updateCart()
        },
        updateCart() {
            localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
        },
        removeFromCart(item) {
            this.$emit('removeFromCart', item)
            this.updateCart()
        }
    },
}
</script>