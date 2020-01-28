<template>
  <div id="app" class="main">
    <div class="header">
      <div class="wrapper wrapper-header">
        <div class="header__title">Lesson 2</div>
      </div>
    </div>
    <div class="wrapper wrapper-main">
      <my-button
        :priority="1"
      />
    </div>
  </div>
</template>

<script>
import myButton from '@/components/myButton.vue'

export default {
  name: 'app',
  components: {
    myButton
  }
}
</script>

<style>

</style>
