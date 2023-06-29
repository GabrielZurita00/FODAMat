<template>
    {{ "Área "+show }}
    <div class="foda">
        <div class="parent counters" v-if="show>0"> 
            <div class="div1">F
                <button @click="subtractF">-</button>
                <div>{{ foda.f }}</div>
                <button @click="addF">+</button>
            </div>
            <div class="div2">O
                <button @click="subtractO">-</button>
                <div>{{ foda.o }}</div>
                <button @click="addO">+</button>
            </div>
            <div class="div3">D
                <button @click="subtractD">-</button>
                <div>{{ foda.d }}</div>
                <button @click="addD">+</button>
            </div>
            <div class="div4">A
                <button @click="subtractA">-</button>
                <div>{{ foda.a }}</div>
                <button @click="addA">+</button>
            </div>
        </div>
        <div class="table">
            <table>
                <tr>
                    <th></th>
                    <th v-for="col in foda.o" :key="col">{{ "O"+col }}</th>
                    <th v-for="col in foda.a" :key="col">{{ "A"+col }}</th>
                    <th>Total</th>
                </tr>
                <tr v-for="row in foda.f" :key="row">
                    <td style="font-weight: bold;">{{ "F"+row }}</td>
                    <td v-for="col in foda.o" :key="col">
                        <input type="number" style="width:30px;">
                    </td>
                    <td v-for="col in foda.a" :key="col">
                        <input type="number" style="width:30px;">
                    </td>
                    <td></td>
                </tr>
                <tr v-for="row in foda.d" :key="row">
                    <td style="font-weight: bold;">{{ "D"+row }}</td>
                    <td v-for="col in foda.o" :key="col">
                        <input type="number" style="width:30px;">
                    </td>
                    <td v-for="col in foda.a" :key="col">
                        <input type="number" style="width:30px;">
                    </td>
                    <td></td>
                </tr>
                <tr>
                    <td style="font-weight: bold;">Total</td>
                    <td v-for="col in foda.o" :key="col"></td>
                    <td v-for="col in foda.a" :key="col"></td>
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
            this.foda = JSON.parse(localStorage.getItem(`Area${newValue}`))
        }
    },
    data() {
      return {
        foda: {
            f: 0,
            o: 0,
            d: 0,
            a: 0
        }
      };
    },
    methods: {
      addF() {
        this.foda.f++;
        localStorage.setItem(`Area${this.show}`, JSON.stringify(this.foda))
      },
      subtractF() {
        this.foda.f--;
        localStorage.setItem(`Area${this.show}`, JSON.stringify(this.foda))
      },
      addO() {
        this.foda.o++;
        localStorage.setItem(`Area${this.show}`, JSON.stringify(this.foda))
      },
      subtractO() {
        this.foda.o--;
        localStorage.setItem(`Area${this.show}`, JSON.stringify(this.foda))
      },
      addD() {
        this.foda.d++;
        localStorage.setItem(`Area${this.show}`, JSON.stringify(this.foda))
      },
      subtractD() {
        this.foda.d--;
        localStorage.setItem(`Area${this.show}`, JSON.stringify(this.foda))
      },
      addA() {
        this.foda.a++;
        localStorage.setItem(`Area${this.show}`, JSON.stringify(this.foda))
      },
      subtractA() {
        this.foda.a--;
        localStorage.setItem(`Area${this.show}`, JSON.stringify(this.foda))
      },
    },
    mounted() {
      if (this.show>0){
        this.foda = JSON.parse(localStorage.getItem(`Area${this.show}`))
      }
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
    .counters{
        grid-area: 1 / 1 / 5 / 2;
        height: 300px;
    }
    .table{
        grid-area: 1 / 2 / 5 / 5;
    }
    table, th, td {
        border:1px solid black;
      }
</style>