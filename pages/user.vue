<template>
<div>
  Name: {{ fullName }}
  Message: {{ message }}
</div>
</template>

<script lang="ts">
import { defineComponent, computed, ref } from '@vue/composition-api'

interface User {
  firstName: string
  lastName: string
}

export default defineComponent({
  
  // props <user :user=
  /*
  props: {
    user: {
      type: Object as () => User,
      required: false
    }
  },
  */

  //setup ({ user }) {
  setup(){
    const user:User={firstName:'satoshi', lastName:'aikawa'}
    //const user = ref<User>({firstName:'satoshi', lastName:'aikawa'})
    const fullName = computed(() => `${user.firstName} ${user.lastName}`)
    const message = ref('This is a message')

    return {
      fullName,
      message
    }
  }
})
</script>