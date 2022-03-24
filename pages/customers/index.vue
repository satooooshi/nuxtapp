<template>
<div>
  <h1>Customer list</h1>
  <ul>
    <li v-for="customer, idx in customers" :key=idx>
        <nuxt-link :to="'/customers/'+customer.id"  >{{ customer.email }}</nuxt-link>
    </li>
  </ul>
</div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref, onMounted, useContext } from '@nuxtjs/composition-api'

export default defineComponent({
    setup() {
        const context = useContext()

        const state = reactive({
          customers: [] as any
        })

        const onClick = () => {
            console.log("onClick")
        }

        const fetchFunc = async () => {
            const url = new URL(
              "https://api.chec.io/v1/customers"
            ); 

            let headers = {
                "X-Authorization": process.env.NUXT_ENV_CHEC_SECRET_API_KEY,
                "Accept": "application/json",
                "Content-Type": "application/json",
            };

          await fetch(url, {
            method: "GET",
            headers: headers,
          })
          .then(response => response.json())
          .then(json => {console.log(json);state.customers=json.data});

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
    layout: 'customers',
})
</script>
