<template>
  <v-app>
    <NavBar/>
    <v-content>
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
import NavBar from '@/components/NavBar.vue'

export default {
  components: { NavBar },
  name:'App',
  data(){

  }
}
</script>
