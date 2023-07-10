<template>
  <h1 v-if="show>0">{{ "Área "+show }}</h1>
  <div class="foda">
      <div class="table">
          <table>
              <tr>
                  <td></td>
                  <td></td>
                  <th :colspan="values.o"> 
                    <button @click="subtractO" v-if="values.o>1">-</button>
                    O 
                    <button @click="addO">+</button>
                  </th>
                  <th :colspan="values.a"> 
                    <button @click="subtractA" v-if="values.a>1">-</button>
                    A 
                    <button @click="addA">+</button>
                  </th>
                  <th rowspan="2">Total</th>
              </tr>
              <tr>
                  <td></td>
                  <td></td>
                  <th v-for="col in values.o" :key="col">{{ "O"+col }}</th>
                  <th v-for="col in values.a" :key="col">{{ "A"+col }}</th>
              </tr>
              <tr v-for="row in values.f" :key="row">
                  <th v-if="row==1" :rowspan="values.f">
                    <button @click="subtractF" v-if="values.f>1">-</button>
                    F
                    <button @click="addF">+</button>
                  </th>
                  <th>{{ "F"+row }}</th>
                  <td v-for="col in values.o" :key="col">
                      <input type="number" style="width:30px;">
                  </td>
                  <td v-for="col in values.a" :key="col">
                      <input type="number" style="width:30px;">
                  </td>
                  <td></td>
              </tr>
              <tr v-for="row in values.d" :key="row">
                <th v-if="row==1" :rowspan="values.d">
                  <button @click="subtractD" v-if="values.d>1">-</button>
                  D
                  <button @click="addD">+</button>
                </th>
                  <th>{{ "D"+row }}</th>
                  <td v-for="col in values.o" :key="col">
                      <input type="number" style="width:30px;">
                  </td>
                  <td v-for="col in values.a" :key="col">
                      <input type="number" style="width:30px;">
                  </td>
                  <td></td>
              </tr>
              <tr>
                  <th colspan="2">Total</th>
                  <td v-for="col in values.o" :key="col"></td>
                  <td v-for="col in values.a" :key="col"></td>
                  <td></td>
              </tr>
          </table>
      </div>
  </div>
    
</template>
  
<script>
  export default {
    name: "FodaInput",
    props: ['show'],
    watch: {
        show: function(newValue){
            this.values = JSON.parse(localStorage.getItem("FODA")).areas[newValue-1]
        }
    },
    data() {
      return {
        values: {
            f: 1,
            o: 1,
            d: 1,
            a: 1
        },
      };
    },
    methods: {
      addF() {
        this.values.f++;
        let foda = JSON.parse(localStorage.getItem("FODA"))
        foda.areas[this.show-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(foda))
      },
      subtractF() {
        this.values.f--;
        let foda = JSON.parse(localStorage.getItem("FODA"))
        foda.areas[this.show-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(foda))
      },
      addO() {
        this.values.o++;
        let foda = JSON.parse(localStorage.getItem("FODA"))
        foda.areas[this.show-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(foda))
      },
      subtractO() {
        this.values.o--;
        let foda = JSON.parse(localStorage.getItem("FODA"))
        foda.areas[this.show-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(foda))
      },
      addD() {
        this.values.d++;
        let foda = JSON.parse(localStorage.getItem("FODA"))
        foda.areas[this.show-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(foda))
      },
      subtractD() {
        this.values.d--;
        let foda = JSON.parse(localStorage.getItem("FODA"))
        foda.areas[this.show-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(foda))
      },
      addA() {
        this.values.a++;
        let foda = JSON.parse(localStorage.getItem("FODA"))
        foda.areas[this.show-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(foda))
      },
      subtractA() {
        this.values.a--;
        let foda = JSON.parse(localStorage.getItem("FODA"))
        foda.areas[this.show-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(foda))
      },
    },
    mounted() {
      this.values = JSON.parse(localStorage.getItem("FODA")).areas[this.show-1]
      
    }
  };
  </script>
  
<style scoped>
  .parent {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(4, 1fr);
    grid-column-gap: 0px;
    grid-row-gap: 0px;
  }

  .div1 { 
    grid-area: 1 / 1 / 2 / 4; 
    display: flex;
    height: 30px;
    justify-content: space-evenly;
  }
  .div2 { 
    grid-area: 2 / 1 / 3 / 4; 
    display: flex;
    height: 30px;
    justify-content: space-evenly;
  }
  .div3 { 
    grid-area: 3 / 1 / 4 / 4; 
    display: flex;
    height: 30px;
    justify-content: space-evenly;
  }
  .div4 { 
    grid-area: 4 / 1 / 5 / 4; 
    display: flex;
    height: 30px;
    justify-content: space-evenly;
  }
  .foda{
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
    grid-column-gap: 0px;
    grid-row-gap: 0px;
  }
  .table{
    grid-area: 1 / 1 / 5 / 5;
  }
  table, th, td {
    border:1px solid black;
  }
  h1 {
    font-size: 3rem;
    font-weight: 800;
    margin: 0;
    color: rgb(var(--blue-accent));
  }
</style>