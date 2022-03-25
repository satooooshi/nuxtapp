<template>
<div>
  <h1>products</h1>
  <ul>
    <li v-for="product, idx in products" :key=idx>
        <nuxt-link :to="'/products/'+product.id"  >{{ product.name }}</nuxt-link>
        <a :href="product.image.url" >External Link to {{product.name}}</a>
         <button href=""  @click="onClick" >detail</button>
    </li>
  </ul>
  <template>
  <v-container class="grey lighten-5">
    <v-row
      :justify="start"
    >
      <v-col
        v-for="product, idx in products"
        :key="idx"
        md="3"
      >
      <product-card/>
      </v-col>
    </v-row>
    <cart-item/>
    <cart-item/>
    <cart-item/>

  </v-container>
</template>
</div>
</template>



<script lang="ts">
import { defineComponent, reactive, toRefs, ref, onMounted, useContext } from '@nuxtjs/composition-api'
import CartItem from '~/components/CartItem.vue'

// grid system
// https://vuetifyjs.com/ja/components/grids/#section-521765b95411306e30e930c330d430f330b0

export default defineComponent({
  components: { CartItem },
    setup() {
        const context = useContext()

        const state = reactive({
          products: [] as any,
        })

        const onClick = () => {
            console.log("onClick")
        }

        const fetchFunc = async () => {
            console.log("fetchProdcuts")
            const url = new URL(
              "https://api.chec.io/v1/products"
            ); 

            let headers = {
                "X-Authorization": process.env.NUXT_ENV_CHEC_PUBLIC_API_KEY,
                "Accept": "application/json",
                "Content-Type": "application/json",
            };

          await fetch(url, {
            method: "GET",
            headers: headers,
          })
          .then(response => response.json())
          .then(json => {console.log(json);state.products=json.data});

        }

        onMounted(() => {
            console.log('mounted!')
            console.log(context)
            fetchFunc()
        })

        return {
            ...toRefs(state),
            onClick,
        }
    },
    layout: 'products',
})
</script>
