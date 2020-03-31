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
import moment from 'moment';
import {Component, Vue} from 'vue-property-decorator';
import {Article} from '@/models/Article';
import axios from 'axios';

@Component({
  components: {
    Articles,
  }
})
export default class App extends Vue {

  articles: Article[] = [];

  mounted() {
    new Promise(resolve => {
      this.loadFromFile().then(load => {
        this.articles = (load as Article[]).sort((a, b) => moment(a.datetime, 'YYYY/MM/DD kk:mm').isBefore(moment(b.datetime, 'YYYY/MM/DD kk:mm')) ? -1 : 1);
      })
      resolve();
    });
  }
  
  async loadFromFile(): Promise<Article[]> {
    return (await axios.get('https://tomk.sakura.ne.jp/covid19history/articles.json')).data as Article[];
  }
}
</script>
