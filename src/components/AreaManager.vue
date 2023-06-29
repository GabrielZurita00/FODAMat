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
    props: {
    },
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
  color: rgb(varvar(--blue-accent)); /* Black text color */
  display: block; /* Make the links appear below each other */
  padding: 12px; /* Add some padding */
  text-decoration: none; /* Remove underline from links */
}

.vertical-menu a:hover {
  background-color: #ccc; /* Dark grey background on mouse-over */
}

.vertical-menu a.active {
  background-color: rgb(var(--blue-accent)); /* Add a green color to the "active/current" link */
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