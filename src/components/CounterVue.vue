<template>
    
    <button @click="subtract">-</button>
    <div>{{ count }}</div>
    <button @click="add">+</button>
</template>
  
<script>
  export default {
    name: "CounterVue",
    props: {
      demo: String,
    },
    data() {
      return {
        count: 0,
      };
    },
    methods: {
      add() {
        this.count++;
      },
      subtract() {
        this.count--;
      },
    },
  };
  </script>
  
  <style scoped></style>