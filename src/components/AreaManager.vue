<template>
<div>
  <div class="parent-area">
    <div class="div1-area">
      <div class="vertical-menu" v-if="count>=0">
        <a href="#" class="active"><h3>Áreas:</h3></a>
        <a href="#" v-for="area in count" :key="area" @click="show(area)">Área {{ area }}
          <button v-if="count>1"  @click="remove(area)">-</button>
        </a>
      </div>
      <button @click="add">Agregar Area +</button>
    </div>
    <div class="div2-area">
      <h1 v-if="selected>0">{{ "Área "+selected }}</h1>
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
                  <tr v-for="(row, rowIndex) in values.f" :key="rowIndex">
                      <th v-if="row==1" :rowspan="values.f">
                        <button @click="subtractF" v-if="values.f>1">-</button>
                        F
                        <button @click="addF">+</button>
                      </th>
                      <th>{{ "F"+row }}</th>
                      <td v-for="(col, colIndex) in values.o" :key="colIndex">
                          <dropdown :options="dropdownOptions" :matrixValue="values.matriz[rowIndex][colIndex]" @option-selected="onOptionSelected(rowIndex, colIndex, $event)" />
                      </td>
                      <td v-for="(col, colIndex) in values.a" :key="colIndex">
                          <dropdown :options="dropdownOptions" :matrixValue="values.matriz[rowIndex][colIndex+values.o]" @option-selected="onOptionSelected(rowIndex, colIndex+values.o, $event)" />
                      </td>
                      <td>{{ rowTotal(rowIndex) }}</td>
                  </tr>
                  <tr v-for="(row, rowIndex) in values.d" :key="rowIndex">
                    <th v-if="row==1" :rowspan="values.d">
                      <button @click="subtractD" v-if="values.d>1">-</button>
                      D
                      <button @click="addD">+</button>
                    </th>
                      <th>{{ "D"+row }}</th>
                      <td v-for="(col, colIndex) in values.o" :key="colIndex">
                          <dropdown :options="dropdownOptions" :matrixValue="values.matriz[rowIndex+values.f][colIndex]" @option-selected="onOptionSelected(rowIndex+values.f, colIndex, $event)" />
                      </td>
                      <td v-for="(col, colIndex) in values.a" :key="colIndex">
                          <dropdown :options="dropdownOptions" :matrixValue="values.matriz[rowIndex+values.o][colIndex+values.o]" @option-selected="onOptionSelected(rowIndex+values.f, colIndex+values.o, $event)" />
                      </td>
                      <td>{{ rowTotal(rowIndex+values.f) }}</td>
                  </tr>
                  <tr>
                      <th colspan="2">Total</th>
                      <td v-for="(col, colIndex) in values.o" :key="colIndex">{{ colTotal(colIndex) }}</td>
                      <td v-for="(col, colIndex) in values.a" :key="colIndex">{{ colTotal(colIndex+values.o) }}</td>
                      <td>{{ total }}</td>
                  </tr>
              </table>
          </div>
      </div>
    </div>
  </div>
    <div v-if="showModal">
      <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-container">

              <div class="modal-header">
                <slot name="header">
                  ¿Está seguro de eliminar el área {{ this.delArea }}?
                </slot>
              </div>

              

              <div class="modal-footer">
                <slot name="footer">
                  <button class="modal-default-button" @click="confirmDelete">
                    Si
                  </button>
                  <button class="modal-default-button" @click="closeModal">
                    No
                  </button>
                </slot>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
</div>
    
</template>
  
<script>
import Dropdown from './Dropdown.vue';
  export default {
    components: { Dropdown },
    name: "AreaManager",
    watch: {
        selected: function(newValue){
            this.values = this.foda.areas[newValue-1]
        }
    },
    data() {
      return {
        count: 1,
        selected: 1,
        foda: { areas: [{
          f: 1,
          o: 1,
          d: 1,
          a: 1,
          matriz: [[0, 0],
                   [0, 0]]
        }] },
        values: {
            f: 1,
            o: 1,
            d: 1,
            a: 1,
            matriz: [[0, 0],
                     [0, 0]]
        },
        showModal: false,
        delArea: 0,
        selectedOption: 0,
        //options: [0, 2, 4, 6, 8, 10],
        dropdownOptions: [0, 2, 4, 6, 8, 10],
        tableData: [[0,0],
                    [0,0]]
      };
    },
    computed: {
      
      total(){
        let sum = 0;
        const rows = this.values.matriz.length
        const cols = this.values.matriz[0].length
        for (let i=0;i<cols;i++){
            for(let j=0;j<rows;j++){
                sum+=this.values.matriz[j][i]
            }
        }
        return sum
      }
    },
    methods: {
      rowTotal(index){
        return this.values.matriz[index].reduce((acc, cell) => acc+cell, 0)
      },
      colTotal(index){
        const rows = this.values.matriz.length
        let sum = 0;
        for (let j=0;j<rows;j++){
          sum+=this.values.matriz[j][index]
        }
        return sum
      },
      add() {
        this.foda.areas.push({
          f:1,
          o:1,
          d:1,
          a:1,
          matriz: [[0, 0],
                   [0, 0]]                                                                                                                                       
        })
        this.count = this.foda.areas.length
        console.log(this.foda);
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      remove(area) {
        this.showModal = true
        this.delArea = area
      },
      show(area){
        this.selected=area
        this.values = this.foda.areas[this.selected-1]
      },
      confirmDelete(){
        let c=0
        if (this.selected===this.delArea && this.delArea>1){ 
          this.selected=this.delArea-1
        }
        if (this.selected===this.delArea && this.delArea===1) {
          c=1
          this.selected=this.delArea+1
        }
        this.foda.areas.splice(this.delArea-1,1)
        this.count = this.foda.areas.length
        console.log(this.foda);
        localStorage.setItem("FODA", JSON.stringify(this.foda))
        if (c===1) {
          this.selected=1
          this.values=this.foda.areas[c-1]
        }
        this.closeModal()
      },
      closeModal(){
        this.showModal=false
        this.delArea=0
      },
      addF() {
        this.values.f++;
        this.values.matriz.splice(this.values.f-1,0,new Array(this.values.o+this.values.a).fill(0))
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      subtractF() {
        this.values.f--;
        this.values.matriz.splice(this.values.f,1)
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      addO() {
        this.values.o++;
        for(let i=0;i<this.values.matriz.length;i++) this.values.matriz[i].splice(this.values.o-1,0,0)
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      subtractO() {
        this.values.o--;
        for(let i=0;i<this.values.matriz.length;i++) this.values.matriz[i].splice(this.values.o,1)
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      addD() {
        this.values.d++;
        this.values.matriz.push(new Array(this.values.o+this.values.a).fill(0))
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      subtractD() {
        this.values.d--;
        this.values.matriz.pop()
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      addA() {
        this.values.a++;
        for(let i=0;i<this.values.matriz.length;i++){
          this.values.matriz[i].push(0)
        }
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      subtractA() {
        this.values.a--;
        for(let i=0;i<this.values.matriz.length;i++){
          this.values.matriz[i].pop()
        }
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      onOptionSelected(row,col,option){
        this.values.matriz[row][col] = option;
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      }
    },
    mounted() {
      if (localStorage.getItem("FODA")!==null){
        this.foda = JSON.parse(localStorage.getItem("FODA"))
      }
      this.count = this.foda.areas.length
      this.selected = 1
      this.values = this.foda.areas[this.selected-1]

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

  .parent-area {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
  }

  .div1-area { grid-area: 1 / 1 / 6 / 3; }
  .div2-area { grid-area: 1 / 3 / 6 / 6; }
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
  .modal-mask {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: table;
    transition: opacity 0.3s ease;
  }
  
  .modal-wrapper {
    display: table-cell;
    vertical-align: middle;
  }
  
  .modal-container {
    width: 300px;
    margin: 0px auto;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
    transition: all 0.3s ease;
    font-family: Helvetica, Arial, sans-serif;
  }
  
  .modal-header h3 {
    margin-top: 0;
    color: #42b983;
  }
  
  .modal-body {
    margin: 20px 0;
  }
  
  .modal-default-button {
    float: right;
  }
  
  /*
   * The following styles are auto-applied to elements with
   * transition="modal" when their visibility is toggled
   * by Vue.js.
   *
   * You can easily play with the modal transition by editing
   * these styles.
   */
  
  .modal-enter {
    opacity: 0;
  }
  
  .modal-leave-active {
    opacity: 0;
  }
  
  .modal-enter .modal-container,
  .modal-leave-active .modal-container {
    -webkit-transform: scale(1.1);
    transform: scale(1.1);
  }
</style>