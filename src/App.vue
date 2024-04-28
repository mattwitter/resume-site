<template>
  <v-app id="app">
    <v-col>
        <transition mode="out-in">
          <router-view :key="$route.path"/>
        </transition>
      </v-col>
    <v-snackbar
      v-model="$root.snackbar.enabled"
      :color="$root.snackbar.color"
      :timeout="$root.snackbar.timeout"
    >
      {{ $root.snackbar.text }}
    </v-snackbar>
  </v-app>
</template>

<script>
export default {
  name: "App",
  
  components: {
  },

  methods: {

  },

  created() {
  }
}

</script>

<style>
@import url('https://fonts.cdnfonts.com/css/w95fa');

html { 
  overflow-y: hidden 
}

.text { 
  font-family: 'W95FA', sans-serif; 
  font-size: 18px; 
}

</style>
