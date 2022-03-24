<template>
  <section>
    <p>cart</p>
  </section>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref, onMounted, useContext } from '@nuxtjs/composition-api'

export default defineComponent({
    setup() {
        const context = useContext()

        const state = reactive({
          cart: [] as any
        })

        const onClick = () => {
            console.log("onClick")
        }

        const fetchFunc = async () => {
            const url = new URL(
              "https://api.chec.io/v1/carts"
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
          .then(json => {console.log(json);state.cart=json.data});

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
    layout: 'cart',
})
</script>
