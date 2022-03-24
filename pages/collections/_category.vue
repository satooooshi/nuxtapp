<template>
<div>
  <h1>collection</h1>
</div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref, onMounted, useContext } from '@nuxtjs/composition-api'

export default defineComponent({
    setup() {
        const context = useContext()

        const state = reactive({
          category: [] as any
        })

        const onClick = () => {
            console.log("onClick")
        }

        const fetchFunc = async () => {
            let categoryId:string=context.route.value.params.category
            const url = new URL(
              "https://api.chec.io/v1/categories/"+categoryId
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
          .then(json => {console.log(json);state.category=json.data})
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
