<template>
  <section>
    <p>{{ $route.params.id }}</p>
  </section>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref, onMounted, useContext } from '@nuxtjs/composition-api'

export default defineComponent({
    setup() {
        const context = useContext()

        const state = reactive({
          product: [] as any
        })

        const onClick = () => {
            console.log("onClick")
        }

        const fetchFunc = async () => {
            console.log("fetchProdcut")
            let productId:string=context.route.value.params.id
            const url = new URL(
              "https://api.chec.io/v1/products/"+productId
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
          .then(json => {console.log(json);state.product=json.data});

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
    layout: 'product',
})
</script>
