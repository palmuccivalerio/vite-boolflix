<script>
import { store } from "../store.js";
export default {
  data() {
    return {
      store,
    };
  },
};
</script>

<template>
  <input type="text" placeholder="scrivi film" v-model="this.store.searched" />
  <button @click="$emit('nameTitle')">cerca</button>
</template>

<style lang="scss" scoped>
input {
  width: 80%;
  background-color: white;
  border-radius: 15px ;
  border: 2px solid black;
  padding: 12px 16px;
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
}

button {
  width: 20%;
  border: 2px solid black;
  border-radius: 15px ; 
  padding: 12px 16px;
  background-color: red;
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
  cursor: pointer;
}
</style>
