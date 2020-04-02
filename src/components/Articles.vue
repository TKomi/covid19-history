<template>
  <v-container>
    <v-row class="my-4">
      <v-col class="py-0">
        <v-select v-model="categoriesSelected" :items="categories" multiple label="カテゴリ"/>
      </v-col>
      <v-col cols="12" sm="2" class="py-0">
        <v-btn @click="onClickAllUpdateCategories" rounded large>全切替</v-btn>
      </v-col>
    </v-row>
    <v-row class="my-4">
      <v-col class="py-0">
       <v-select v-model="placesSelected" :items="places" multiple label="場所"/>
      </v-col>
      <v-col cols="12" sm="2" class="py-0">
        <v-btn @click="onClickAllUpdatePlaces" rounded large>全切替</v-btn>
      </v-col>
    </v-row>
    <v-row class="my-4">
      <v-col class="py-0">
        <v-select v-model="displayOrderSelected" :items="displayOrders" label="並び順"/>
      </v-col>
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
  import moment from 'moment';

  @Component({
    components: {
      Article,
    }
  })
  export default class Articles extends Vue{

    @Prop(Array) articles!: ArticleModel[];

    displayOrderSelected = '日付 降順';
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
        .filter(article => this.placesSelected.some(place => article.place.includes(place)))
        .sort(this.displayOrderFunction);
    }

    get displayOrders(): string[] {
      return [
        '日付 降順',
        '日付 昇順'
      ]
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

    displayOrderFunction(left: ArticleModel, right: ArticleModel): number {
      switch(this.displayOrderSelected) {
        case '日付 降順':
          return moment(left.datetime, 'YYYY-MM-DD kk:mm')
            .isBefore(moment(right.datetime, 'YYYY-MM-DD kk:mm')) 
            ? 1
            : -1;
        case '日付 昇順':
        default: 
          return moment(left.datetime, 'YYYY-MM-DD kk:mm')
            .isBefore(moment(right.datetime, 'YYYY-MM-DD kk:mm')) 
            ? -1
            : 1;
      }
    }

  }
</script>
