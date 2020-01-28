<template>
  <div id="app" class="main">
    <div class="header">
      <div class="wrapper wrapper-header">
        <div class="header__title">Lesson 2</div>
      </div>
    </div>
    <div class="wrapper wrapper-main">
      <my-button
        :priority="25"
        :left="0"
        :top="100"
      />
      <my-button
        :priority="28"
        :left="100"
        :top="300"
      />
      <my-button
        :priority="32"
        :left="500"
        :top="222"
      />
      <my-button
        :priority="48"
        :left="300"
        :top="500"
      />
      <my-button
        :priority="77"
        :left="400"
        :top="50"
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
