<template>
      <div class="inputTag">
        <div class="tags">
            <div class="tag" v-for="(tag, index) in tags" :key="index">
                {{ tag }} <button >X</button>
            </div>
        </div>
        <form >
          <input class="input" type="text" >
        </form>


    </div>
</template>

<script>
export default {

    data() {
        return {
            currentValue: "",
            tags: [],
        };
  },
  methods:{

    handleSubmit(){
        if (this.currentValue != ""){
            const exist = this.tags.some( item => item == this.currentValue);
            if (!exist)
            this.tags.push(this.currentValue);
            this.currentValue="";
        }       
    }


  },

};
</script>

<style>

</style>