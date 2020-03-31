<template>
  <v-container>
    <v-row>
      <v-select v-model="categoriesSelected" :items="categories" multiple label="カテゴリ"/>
      <v-btn @click="onClickAllUpdateCategories" rounded large>全切替</v-btn>
    </v-row>
    <v-row>
      <v-select v-model="placesSelected" :items="places" multiple label="場所"/>
      <v-btn @click="onClickAllUpdatePlaces" rounded large>全切替</v-btn>
    </v-row>
    <v-timeline dense>
      <Article v-for="article in articlesComputed" 
        :key="article.title" 
        :title="article.title" 
        :datetime="article.datetime" 
        :source="article.source"
        :category="article.category"
        :href="article.URL"
        :important="article.important"
        class="my-2"
      />
    </v-timeline>
  </v-container>
</template>

<script lang="ts">
  import Article from './Article.vue';
  import {Component, Vue, Prop, Watch} from 'vue-property-decorator';
  import {Categories} from '@/models/Categories';
  import {Article as ArticleModel} from '@/models/Article';

  @Component({
    components: {
      Article,
    }
  })
  export default class Articles extends Vue{

    @Prop(Array) articles!: ArticleModel[];

    displayOrder = 'datetime-desc';
    categoriesSelected: string[] = [];
    placesSelected: string[] = [];

    get categories() {
      return Categories;
    }

    get places() {
      return [...new Set(this.articles.flatMap(article => article.place.split(',')).map(text => text.trim()))];
    }

    get articlesComputed() {
      return this.articles
        .filter(article => this.categoriesSelected.includes(article.category ? article.category : ''))
        .filter(article => this.placesSelected.some(place => article.place.includes(place)));
    }

    @Watch('articles')
    setAllPlaces() {
      this.placesSelected.push(...this.places);
    }

    setAllCategories() {
      this.categoriesSelected.push(...this.categories);
    }

    mounted() {
      this.setAllCategories();
    }

    onClickAllUpdateCategories() {
      if(this.categoriesSelected.length > 0) {
        this.$set(this, 'categoriesSelected', []);
      } else {
        this.setAllCategories();
      }
    }

    onClickAllUpdatePlaces() {
      if(this.placesSelected.length > 0) {
        this.$set(this, 'placesSelected', []);
      } else {
        this.setAllPlaces();
      }
    }

  }
</script>
