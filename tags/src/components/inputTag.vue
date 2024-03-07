<template>
    <div class="inputTag">
        <div class="tags">
            <div class="tag" v-for="(tag, index) in tags" :key="index">
                {{ tag }} <button @click="deleteTag(tag)">X</button>
            </div>
        </div>
        <form @submit.prevent="handleSubmit">
          <input class="input" type="text" v-model="currentValue"
           @keydown="handlekeydown">
        </form>
        <!--<label>Input 2</label><input type="text"
             v-model="currentValue" @keydown="handlekeydown"> -->

    </div>
</template>

<script>
export default {
    emits: ["onTagsChange"],
    data() {
        return {
            currentValue: "",
            tags: [],
        };
  },
  methods:{

    deleteTag(tag){
        this.tags = this.tags.filter(item => item != tag);
        this.$emit('onTagsChange',this.tags);
    },
     
    handlekeydown(e){

        if (e.key == "Backspace" && this.currentValue == ""){
            this.tags.pop();
            this.$emit('onTagsChange',this.tags);
        }
    },
    handleSubmit(){
        if (this.currentValue != ""){
            const exist = this.tags.some( item => item == this.currentValue);
            if (!exist)
            this.tags.push(this.currentValue);
            this.currentValue="";
            this.$emit('onTagsChange',this.tags);
        }       
    }


  },

};
</script>

<style>
.inputTag {
    display: inline-flex;
    border: solid 1px #000;
    border-radius: 3px;
    height: 43px;
}

.tags {
    display: flex;
    gap: 3px;
    padding: 5px;
}
.tags .tag{
    display: flex;
    padding: 5px;
    border: solid 1px #ccc;
    gap: 5px;
    align-content: center;
    border-radius: 3px;
}

.inputTag .input{
    border: none;
    outline: none;
    padding: 0 5px;
}
.inputTag form{
    display: inline-flex;
}

.tag button{
    background-color: transparent;
    border: none;
    border-radius: 3px;
    cursor: pointer;
}
.tag button:hover{
    background-color: #ccc;
}

</style>