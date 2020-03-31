<template>
  <v-timeline-item :color="categoryColor" class="py-0" :small="!important">
    <v-card :href="href" target="_blank">
      <v-card-title class="px-3 py-1">
        {{title}}
      </v-card-title>
      <v-card-text class=" px-3 pb-1 pt-0">
        {{datetime | datetimeformat}}
        {{source}}
      </v-card-text>
    </v-card>
  </v-timeline-item>
</template>

<script lang="ts">
import {Component, Prop, Vue} from 'vue-property-decorator';
import moment from 'moment';

@Component({
  filters: {
    datetimeformat: function(dt: string) {
      return moment(dt, 'YYYY/MM/DD kk:mm').format('YYYY-MM-DD');
    }
  }
})
export default class Article extends Vue {
  @Prop({type: String, default: ''}) title!: string;

  @Prop(String) datetime?: string;

  @Prop(String) source?: string;

  @Prop(String) category?: string;

  @Prop(String) href?: string;

  @Prop({type: Boolean, default: false}) important?: boolean;

  get categoryColor() {
    switch(this.category) {
      case '感染':
        return 'red';
      case '政治':
        return 'pink';
      case '経済':
        return 'purple';
      case '差別・デマ':
        return 'cyan';
      case '治療・対策':
        return 'teal';
      case '症状':
        return 'green';
      case 'ウイルス':
        return 'lime'
      case '著名人':
        return 'yellow';
      case 'WHO':
        return 'orange';
      case 'クルーズ船':
        return 'brown';
      case '検疫':
        return 'light-green';
      case '教育':
        return 'blue';
      default: 
        return 'grey';
    }
  }
}
</script>