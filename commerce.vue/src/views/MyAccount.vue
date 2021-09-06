<template>
    <div class="page-my-account">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">My account</h1>
            </div>
        </div>
        <div class="column is-12">
            <button @click="logout()" class="button is-danger">Log out</button>
        </div>

        <div class="column">
            <h2 class="subtitle">My orders</h2>
            <OrderSummary
                v-for="order in orders"
                :key="order.id"
                :order="order"
            />
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import OrderSummary from '@/components/OrderSummary.vue'
export default {
    name: 'MyAccount',
    data() {
        return {
            orders:[]
        }
    },
    components: {
        OrderSummary
    },
    mounted() {
        document.title = "My account | PCgaming"
        this.getMyOrders()
    },
    methods: {
        async getMyOrders() {
            this.$store.commit('setIsLoading', true)

            await axios
                .get('/api/v1/orders')
                .then(response => {
                    this.orders = response.data
                })
                .catch(error => {
                    console.log('error',error);
                })
            this.$store.commit('setIsLoading', false)
        },
        logout() {
            axios.defaults.headers.common["Authorization"] = ""
            localStorage.removeItem("token")
            localStorage.removeItem("username")
            localStorage.removeItem("userid")
            this.$store.commit("removeToken")
            this.$router.push("/")
        }
    }
}
</script>