

<script>
import InputTag from './components/inputTag.vue'


export default {
  components: { 
    InputTag,
   },

  methods:{
    handleOnTagsChange(tags){
      console.log(tags)
    },
  },


};
</script>

<template>
  <inputTag @onTagsChange="handleOnTagsChange"/> Input component
  
</template>

<style>

</style>