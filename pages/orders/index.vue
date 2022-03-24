<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field
      v-model="email"
      :rules="emailRules"
      label="E-mail"
      required
    ></v-text-field>

    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="fetchCustomer"
    >
      Fetch Order Histories
    </v-btn>
    {{orders}}

  </v-form>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref, onMounted, useContext } from '@nuxtjs/composition-api'

export default defineComponent({
    setup() {
        const context = useContext()

        const state = reactive({
            valid: true as boolean, // v-form v-model="valid"
            email: '' as string,
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+/.test(v) || 'E-mail must be valid',
            ],
            orders: {} as any
        })

        onMounted(() => {
            console.log('mounted!')
            console.log(context)
        })

        const onClick = () => {
            console.log("onClick")
        }

        // Issue and return login token
        const fetchCustomer = async () => {
            console.log('fetchCustomer')
            if(state.email=='')return

            let customerId:string;

            const url = new URL(
            "https://api.chec.io/v1/customers/issue-token"
            );

            let headers = {
                "X-Authorization": process.env.NUXT_ENV_CHEC_SECRET_API_KEY,
                "Content-Type": "application/json",
                "Accept": "application/json",
            };

            let body = {
                "email": state.email,
                "base_url": "omnis"
            }

            await fetch(url, {
                method: "POST",
                headers: headers,
                body: JSON.stringify(body)
            })
            .then(response => response.json())
            .then(json => {
                console.log(json)
                customerId=json.customer_id
                fetchFunc(customerId)
            });
        }

        
        const fetchFunc = async (customerId:string) => {
            const url = new URL(
              "https://api.chec.io/v1/customers/"+customerId+"/orders"
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
          .then(json => {
              console.log(json);
              state.orders=json
              // if json.meta.total===0 then no orders before
          });
        }




        return {
            ...toRefs(state),
            fetchCustomer,
            onClick,
        }
    },
    layout: 'cart',
})
</script>
