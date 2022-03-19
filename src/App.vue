<template>
  <v-app class="grey">
    <nav-bar></nav-bar>
    <v-main>
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </v-main>
  </v-app>
</template>

<script>
import NavBar from '@/components/NavBar.vue';

export default {
  components: { NavBar},
  name:'App',
  data(){

  }
}
</script>
