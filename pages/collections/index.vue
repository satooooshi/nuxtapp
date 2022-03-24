<template>
<div>
  <h1>Collections</h1>
  <ul>
    <li v-for="category, idx in categories" :key=idx>
        <nuxt-link :to="'/collections/'+category.id"  >{{ category.name }}</nuxt-link>
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
          categories: [] as any
        })

        const onClick = () => {
            console.log("onClick")
        }

        const fetchFunc = async () => {
            const url = new URL(
              "https://api.chec.io/v1/categories"
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
          .then(response => {
            if (!response.ok) {
              console.error('response NOT ok');
              console.error(response) //{ statusCode: response.status, message: response.statusText }
              throw new Error(response.statusText)
            }
            return response.json()
          })
          .then(json => {console.log(json);state.categories=json.data})
          .catch(err=>{
             console.error('Fetch API ネットワークエラー || error thrown because response NOT ok');
             return context.error({ statusCode: undefined, message: err })
             //return context.error({ statusCode: 404, message: 'err message' })
          })

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
