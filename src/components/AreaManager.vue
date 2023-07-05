<template>
  <div class="parent">
    <div class="div1">
      <div class="vertical-menu" v-if="count>=0">
        <a href="#" class="active"><h3>Áreas:</h3></a>
        <a href="#" v-for="area in count" :key="area" @click="show(area)">Área {{ area }}
          <button v-if="count>1"  @click="remove(area)">-</button>
        </a>
      </div>
      <button @click="add">Agregar Area +</button>
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
        count: 1,
        selected: 1,
        foda: { areas: [{
          f: 1,
          o: 1,
          d: 1,
          a: 1
        }] }
      };
    },
    methods: {
      add() {
        this.foda.areas.push({
          f:1,
          o:1,
          d:1,
          a:1
        })
        this.count = this.foda.areas.length
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      remove(area) {
        if (this.selected===area && area>=1) this.selected=area-1
        if (this.selected===area && area===1) this.selected=area+1
        this.foda.areas.splice(area-1,1)
        this.count = this.foda.areas.length
        localStorage.setItem("FODA", JSON.stringify(this.foda))
        this.foda = JSON.parse(localStorage.getItem("FODA"))
      },
      show(area){
        this.selected=area
      }
    },
    mounted() {
      if (localStorage.getItem("FODA")!==null){
        this.foda = JSON.parse(localStorage.getItem("FODA"))
      }
      this.count = this.foda.areas.length
      this.selected = 1
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