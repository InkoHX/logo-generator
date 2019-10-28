<template>
  <v-app id="app">
    <v-app-bar app>
      <v-toolbar-title>IconGenerator</v-toolbar-title>
    </v-app-bar>

    <v-container>
      <v-content>
        <Generator/>
      </v-content>
    </v-container>
  </v-app>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'
const Generator = () => import('@/components/Generator.vue')

@Component({
  components: {
    Generator
  }
})
export default class App extends Vue {}
</script>
