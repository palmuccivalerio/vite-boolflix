<script>
import { store } from "../store.js";
export default {
    data(){
        return{
            store 
        }
    }
}

</script>

<template>
<input type="text" placeholder="scrivi film" v-model="this.store.searched">
<button @click="$emit('nameTitle')" > cerca </button>


</template>



<style>

</style>