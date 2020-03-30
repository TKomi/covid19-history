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
import Articles from './components/Articles.vue';
import articles from '@/assets/articles.json';
import moment from 'moment';
import {Component, Vue} from 'vue-property-decorator';

@Component({
  components: {
    Articles,
  }
})
export default class App extends Vue {

  articles: any[] = [];

  mounted() {
    this.articles = articles.sort((a, b) => moment(a.datetime).isBefore(moment(b.datetime)) ? -1 : 1);
  }
}
</script>
