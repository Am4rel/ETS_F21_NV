<template>
  <v-app light>
    <notifications-bar>
      <v-main :style="{'height':'100%'}"> 
        <v-progress-linear
        v-show="pageLoading"
        :size="50"
        color="primary"
        indeterminate/>
        <Nuxt />
      </v-main>
    </notifications-bar>
  </v-app>
</template>

<script>
import { Vue } from 'nuxt-property-decorator'
import Component from 'nuxt-class-component'

import NotificationsBar from '../components/NotificationBar.vue'

import headLayoutMixin from '~/helpers/mixins/headLayoutMixin'

@Component({
  components: {
    NotificationsBar
  },
  mixins: [headLayoutMixin]
})

export default class EmptyPage extends Vue {
  pageLoading = true;

  mounted() {
    this.pageLoading = false;
  }
}
</script>

<style scoped>
div.v-progress-holder {
  padding: 100px;
  width: 100%;
  display: flex;
  justify-content: center;
}
</style>
