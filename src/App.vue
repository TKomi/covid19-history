<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-toolbar-title>
        COVID-19 History
      </v-toolbar-title>
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
