<template>
  <v-list-item 
  class="mb-4 menu-link"  
  :to="link" 
  color="primary"
  @click="onMenuLinkClick">
    <v-icon>{{ icon }}</v-icon>
    <v-icon 
    color="primary"
    :class="{'icon-circle-half': true, 'd-none': halfCircleDisplayNoneBool}" 
    x-small>mdi-circle-half</v-icon>
    <v-list-item-content class="mx-4 font-weight-light">
      <v-list-item-title >{{ name }}</v-list-item-title>
    </v-list-item-content>
  </v-list-item>
</template>

<script>
import { Prop, Vue } from 'nuxt-property-decorator'
import Component from 'nuxt-class-component'
import { Emit } from 'vue-property-decorator'

@Component({})

export default class MenuLink extends Vue {
  @Prop({type: String, required: true}) icon
  @Prop({type: String, required: true}) name
  @Prop({type: String}) link 
  @Prop({type: Boolean, required: true}) halfCircleDisplayNone

  halfCircleDisplayNoneBool = true;

  mounted() {
    this.halfCircleDisplayNoneBool = this.halfCircleDisplayNone;
  }

  changeHalfCircleDisplay(val) {
    this.halfCircleDisplayNoneBool = val;
  }

  @Emit('onMenuLinkClick')
  onMenuLinkClick() {
    return this.link;
  }
}
</script>

<style scoped>
.icon-circle-half {
  transform: rotate(180deg);
  position: absolute;
  left: -18px;
  box-shadow: inset 0 0 0px 10px #64B5F6,
              0 0 3px 6px #bcdaf3;
  border-radius: 50%;
}

.menu-link:hover {
  background-color: rgba(165, 165, 165, 0.2);
}

.menu-link {
  border-radius: 10px;
}

.menu-link {
  transition: background-color 1s ease;
}
</style>
