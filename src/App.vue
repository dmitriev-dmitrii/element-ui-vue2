<template>
  <div id="app">
    <TagsInput />
  </div>
</template>

<script>
import _get from "lodash/get";
import TagsInput from './components/TagsInput.vue'

export default {
  name: 'App',
  components: {
    TagsInput
  },
  mounted  (){
    console.log(_get(this,'hui' ));
  }
}
</script>

<style>

</style>
