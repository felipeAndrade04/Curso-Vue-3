<template>
  <modal-factory />
  <router-view/>
</template>

<script>
import { watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'

import ModalFactory from './components/ModalFactory'
import services from './services'

export default {
  components: { ModalFactory },
  setup () {
    const route = useRoute()
    const router = useRouter()

    watch(() => route.path, async () => {
      if (route.meta.hasAuth) {
        const token = window.localStorage.getItem('token')
        if (!token) {
          router.push({ name: 'Home' })
          return
        }

        const data = await services.users.getMe()
        console.log(data)
      }
    })
  }
}
</script>
