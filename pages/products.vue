<template>
    <v-container fluid>
        <v-row>
            <v-col>
                <v-textarea
                    v-model="text"
                    hint="Text here"
                    ></v-textarea>
                <br />
                <v-btn @click="onSubmit">
                    Submit
                </v-btn>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <Note
                    :text="shown_text"
                    @clear="onClear"
                    />
            </v-col>
        </v-row>
    </v-container>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, useContext} from '@nuxtjs/composition-api'
import { NuxtAxiosInstance } from '@nuxtjs/axios'

export default defineComponent({
    setup() {
        const products = ref<any[]>([])

        const getUserRepositories = async () => {
            //const response = await NuxtAxiosInstance.$get('https://httpbin.org/get');
            //console.log(response)

        }

        const onSubmit = () => {
            console.log("onsubmit")
        }

        const onClear = () => {
            console.log("onclear")
        }

        const { $config } = useContext()

        onMounted(() => {
            console.log('mounted!')
            console.log($config.stage)
        })

        return {
            products,
            onSubmit,
            onClear,
        }
    },
    layout: 'note',
})
</script>