<template>
    <div class="page-search">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">
                    Search
                </h1>
                <h2 class="is-size-5 has-text-grey">
                    Search term: "{{ query }}"
                </h2>
            </div>
            <ProductComponent
                v-for="product in products"
                :key="product.id"
                :product="product"
            />
        </div>
    </div>
</template>
<script>
import axios from 'axios'
import ProductComponent from '@/components/ProductComponent'
export default {
    name: "Search",
    components: {
        ProductComponent
    },
    data() {
        return{
            products: [],
            query: ''
        }
    },
    mounted() {
        document.title = 'Search | PCgaming'
        let url = window.location.search.substring(1)
        let urlSearchParams = new URLSearchParams(url)
        let params = Object.fromEntries(urlSearchParams.entries())
        console.log('',params);
        if(params.hasOwnProperty('query')){
            this.query = params['query']
            this.performSearch()
        }
    },
    methods: {
        performSearch(){
            
        }
    }
}
</script>