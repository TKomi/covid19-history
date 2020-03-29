<template>
  <v-timeline-item :color="categoryColor">
    <v-card :href="href" target="_blank">
      <v-card-title>
        {{title}}
      </v-card-title>
      <v-card-text>
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
      return moment(dt).format('YYYY-MM-DD');
    }
  }
})
export default class Article extends Vue {
  @Prop({type: String, default: ''}) title!: string;

  @Prop(String) datetime?: string;

  @Prop(String) source?: string;

  @Prop(String) category?: string;

  @Prop(String) href?: string;

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
      default: 
        return 'grey';
    }
  }
}
</script>