<template>
  <div v-if="details">
    <img
      v-if="details.avatar"
      :src="details.avatar"
      :class="size" 
      class="rounded-xl width"
      alt="avatar"
    >
    <div 
      v-else
      :class="size"
      class="ava
      ava-span 
      d-flex 
      justify-center 
      align-center 
      rounded-circle" 
      > 
        <span :class="size"  
          class="box 
          d-flex 
          align-center 
          justify-center">
          {{ details.nickname && details.nickname[0] }}
        </span>
    </div>
  </div>
  
</template>

<script>
import Component, { namespace } from 'nuxt-class-component'
import { Vue, Prop } from 'vue-property-decorator'

const {State, Action} = namespace('user')

@Component({})

export default class Avatar extends Vue{
  @State details
  @Prop({ type: String, required: true}) size

  box = 'box'
}
</script>

<style scoped>

  .small-size.box{
    width: 35px;
    height: 35px;
    font-size: 24px;
    /* min-width: 35px; */
  }

  /* .small-size.width {
    min-width: 35px;
  } */

  .ava {
    display: block;
    background-color: rgba(0, 0, 0, 0.3);
    width: 100%; 
    height: auto;
    font-weight: bold;
  }

  .ava-span.ava {
    margin-right: auto;
    margin-left: auto;
    
  }

  .ava-span.ava.big-size {
    width: 180px
  }

  .width.big-size {
    width: 100%; 
    min-width: 180px;
    height: auto;
  }

    .big-size.box{
    width: 180px;
    height: 180px;
    font-size: 90px;
  }

  /* @media (max-width: 959px) {
    .width {
    width: 200px; 
  }
  } */

  .big-size{

    font-size: 90px;
  }

  .small-size{
    width: auto;
    height: 35px; 
    font-size: 24px;
  }
</style>
