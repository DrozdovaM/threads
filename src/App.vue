<template>
  <div id="app">
    <v-app id="inspire">
      <Application></Application>
    </v-app>
  </div>
</template>

<script>
import Application from "@/components/Application";

export default {
  name: 'App',
  components: {
    Application
  }
}
</script>