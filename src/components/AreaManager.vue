<template>

    <div class="vertical-menu" v-if="count>=0">
        <a href="#" class="active"><h3>Áreas:</h3></a>
        <a href="#" v-for="area in count" :key="area" @click="show(area)">Área {{ area }}</a>
    </div>
    <button @click="subtract">-</button>
    <button @click="add">+</button>
</template>
  
<script>
  export default {
    name: "AreaManager",
    props: {
    },
    data() {
      return {
        count: 0,
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
        let foda = JSON.parse(localStorage.getItem(`Area${area}`))
        console.log(foda);

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
</style>