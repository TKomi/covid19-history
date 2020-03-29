<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        Covid-19
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <Articles :articles="articles"/>
    </v-content>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import Articles from './components/Articles.vue';
import articles from './data/articles.json';

export default Vue.extend({
  name: 'App',

  components: {
    Articles,
  },

  data: () => ({
    articles: articles
  }),
});
</script>
