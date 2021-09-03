<template>
    <div class="page-category">
        <div class="columns is-multiline">
            <div class="column is-12">
                <div class="has-text-centered is-size-2">{{ category.name }}</div>
            </div>
            <ProductComponent
                v-for="product in category.products"
                :key="product.id"
                :product="product"
            />
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
import ProductComponent from '@/components/ProductComponent'
export default{
    name:'Category',
    components:{
        ProductComponent
    },
    data() {
        return {
            category:{
                products:[]
            }
        }
    },
    mounted(){
        this.getCategory()
    },
    watch:{
        $route(to, from) {
            console.log('to',to);
            console.log('from',from); 
            if (to.name === 'Category') {
                this.getCategory()
            }
        }
    },
    methods: {
        async getCategory() {
            const categorySlug = this.$route.params.category_slug
            this.$store.commit('setIsLoading', true)
            
            await axios
                .get(`/api/v1/products/${categorySlug}`)
                .then(response => {
                    this.category = response.data
                    document.title = this.category.name+' | PCgaming'
                })
                .catch(error =>{
                    console.log('error',error);
                    toast({
                        message: 'Error, please try again',
                        type: 'is-danger',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'bottom-right',
                    })
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>