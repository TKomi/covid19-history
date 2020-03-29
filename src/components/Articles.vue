<template>
  <v-container>
    <v-row>
      <v-switch v-model="kansen" label="感染" class="mx-2 my-0"/>
      <v-switch v-model="seiji" label="政治" class="mx-2 my-0"/>
      <v-switch v-model="keizai" label="経済" class="mx-2 my-0"/>
      <v-switch v-model="dema" label="差別・デマ" class="mx-2 my-0"/>
      <v-switch v-model="chiryou" label="治療・対策" class="mx-2 my-0"/>
      <v-switch v-model="shoujou" label="症状" class="mx-2 my-0"/>
      <v-switch v-model="virus" label="ウイルス" class="mx-2 my-0"/>
      <v-switch v-model="chomeijin" label="著名人" class="mx-2 my-0"/>
      <v-switch v-model="who" label="WHO" class="mx-2 my-0"/>
      <v-switch v-model="cruise" label="クルーズ船" class="mx-2 my-0"/>
    </v-row>
    <v-timeline dense>
      <Article v-for="article in articlesComputed" 
        :key="article.title" 
        :title="article.title" 
        :datetime="article.datetime" 
        :source="article.source"
        :category="article.category"
        :href="article.URL"
        class="my-2"
      />
    </v-timeline>
  </v-container>
</template>

<script lang="ts">
  import Vue from 'vue'
  import Article from './Article.vue';

  export default Vue.extend({
    name: 'Articles',

    components: {Article},

    props: [
      'articles'
    ],

    computed: {
      articlesComputed() {
        return this.articles
          .filter((article: any) => (this.kansen && article.category === '感染') || article.category !== '感染')
          .filter((article: any) => (this.seiji && article.category === '政治') || article.category !== '政治')
          .filter((article: any) => (this.keizai && article.category === '経済') || article.category !== '経済')
          .filter((article: any) => (this.dema && article.category === '差別・デマ') || article.category !== '差別・デマ')
          .filter((article: any) => (this.chiryou && article.category === '治療・対策') || article.category !== '治療・対策')
          .filter((article: any) => (this.shoujou && article.category === '症状') || article.category !== '症状')
          .filter((article: any) => (this.virus && article.category === 'ウイルス') || article.category !== 'ウイルス')
          .filter((article: any) => (this.chomeijin && article.category === '著名人') || article.category !== '著名人')
          .filter((article: any) => (this.who && article.category === 'WHO') || article.category !== 'WHO')
          .filter((article: any) => (this.cruise && article.category === 'クルーズ船') || article.category !== 'クルーズ船')
        ;
      }
    },

    data: () => ({
      kansen: true,
      seiji: true,
      keizai: true,
      dema: true,
      chiryou: true,
      shoujou: true,
      virus: true,
      chomeijin: true,
      who: true,
      cruise: true,
    })
  })
</script>
