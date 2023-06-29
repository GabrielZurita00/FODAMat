<template>
  <div class="parent">
    <div class="div1">
      <div class="vertical-menu" v-if="count>=0">
        <a href="#" class="active"><h3>Áreas:</h3></a>
        <a href="#" v-for="area in count" :key="area" @click="show(area)">Área {{ area }}</a>
      </div>
      <button @click="subtract">-</button>
      <button @click="add">+</button>
    </div>
    <div class="div2">
      <FodaInput :show="selected"/>
    </div>
  </div>
    
</template>
  
<script>
import FodaInput from './FodaInput.vue'

  export default {
    name: "AreaManager",
    components: { FodaInput },
    data() {
      return {
        count: 0,
        selected: 0
      };
    },
    methods: {
      add() {
        this.count++;
        localStorage.setItem("areas", this.count)
        const foda = {
            f: 0,
            o: 0,
            d: 0,
            a: 0
        }
        localStorage.setItem(`Area${this.count}`, JSON.stringify(foda))
      },
      subtract() {
        this.count--;
        localStorage.setItem("areas", this.count)
        localStorage.removeItem(`Area${this.count+1}`)
      },
      show(area){
        this.selected=area
      }
    },
    mounted() {
      if (localStorage.getItem("areas")!==null)  this.count = parseInt(localStorage.getItem("areas"))
    }
  };
  </script>
  
<style scoped>
  .vertical-menu {
  width: 200px; 
  align-content: left;
  }

  .vertical-menu a {
  background-color: #eee; 
  color: rgb(varvar(--blue-accent)); 
  display: block; 
  padding: 12px; 
  text-decoration: none; 
  }

  .vertical-menu a:hover {
  background-color: #ccc; 
  }

  .vertical-menu a.active {
  background-color: rgb(var(--blue-accent)); 
  color: white;
  }

  .parent {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
  }

  .div1 { grid-area: 1 / 1 / 6 / 3; }
  .div2 { grid-area: 1 / 3 / 6 / 6; }
</style>