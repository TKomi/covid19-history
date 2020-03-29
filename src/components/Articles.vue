<template>
  <v-container>
    <v-timeline dense>
      <v-timeline-item v-for="article in articles" :key="article.title">
        {{article.title}}
        ({{article.datetime | datetime}})
      </v-timeline-item>
    </v-timeline>
  </v-container>
</template>

<script lang="ts">
  import Vue from 'vue'
  import moment from 'moment';

  export default Vue.extend({
    name: 'Articles',

    props: [
      'articles'
    ],
    filters: {
      datetime: function(dt: string) {
        return moment(dt).format('YYYY-MM-DD');
      }
    }
  })
</script>
