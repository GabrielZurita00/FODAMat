<template>
<div>
  <div class="main-div">
    <div class="logo">
      <a href="/">
        <img src="/512.png" alt="Logo DUEA" width="120" height="120">
      </a>
      <p class="logo-text">FODA Matemático</p>
    </div>
    <div class="area-manager">
      <div class="vertical-menu" v-if="count>=0">
        <div class="area-name">
          <h2>Áreas</h2>
        </div>
        <div class="area-name area-selection" v-for="area in count" :key="area" @click="showArea(area)" v-bind:class="(area!=selected)?'selected-area':''" >
          <h2>Área {{ area }}</h2>
          <div class="remove-area">
            <button class="remove-button" v-if="count>1"  @click="removeArea(area)">
              <img src="/rmbtn.png" alt="removearea" height="25" width="25">
            </button>
          </div>
        </div>
      </div>
      <div class="area-name add-area" @click="addArea">
        <button class="add-button">
          <img src="/addbtn.png" alt="addarea" width="25" height="25">
        </button>
        <h2>Agregar Área</h2>
      </div>
      <!--añadir funcion @click que muestre datos totales-->
      <div class="total-area">
        <h2>TOTAL</h2>
      </div>
    </div>
    <div class="area-info">
      <h1>{{ this.showGraph ? 'Gráficos FODA' : 'Tabla FODA' }}</h1>
      <div class="area-name" @click="showGraph=!showGraph">
        <h2 v-if="selected>0">
          {{ "Área "+selected }}
          <svg v-if="!showGraph" width="28" height="28" viewBox="0 0 52 53" fill="none" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <rect width="52" height="53" fill="url(#pattern0)"/>
            <defs>
            <pattern id="pattern0" patternContentUnits="objectBoundingBox" width="1" height="1">
            <use xlink:href="#image0_86_343" transform="matrix(0.00471866 0 0 0.00462963 -0.00961538 0)"/>
            </pattern>
            <image id="image0_86_343" width="216" height="216" xlink:href="data:image/png;base64,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"/>
            </defs>
          </svg>   
          <svg v-if="showGraph" width="30" height="30" viewBox="0 0 53 53" fill="none" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <rect width="53" height="53" fill="url(#pattern0)"/>
            <defs>
            <pattern id="pattern0" patternContentUnits="objectBoundingBox" width="1" height="1">
            <use xlink:href="#image0_97_142" transform="scale(0.0119048)"/>
            </pattern>
            <image id="image0_97_142" width="84" height="84" xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFQAAABUCAYAAAAcaxDBAAACtklEQVR4nO3cPWsUURjF8b+SwkUCIqiJaBrfIkEQFGNnCrXwU0jUSgsFsbEMfgEtFOwk+Q4WFtYWySoLvoIgGDSFmCKEBCUWibDcHXSeu2cys3B+cIu5yTz77MnMZHaYHTAzMzMzMzOr1I4Kao4DIxXUrcI34F3dTRS5CzwHloGNARvLW73flqeSYQLoUH8oqtEGTkgTCrhWosFBHVeFOZUyDqwJGm/qWANO5gST+09pATidzH0GHgMfgZ//Wf8+cLFr+QXwILOXfuwBjgM3gbHkZ/PAme1o4gq9f9FnQCtQYzZZf1bcY1SL3p42gMvRQjszXnwqWe4A14HVQI0qTtf6sQpMA++T+QvRQjmBpi/yCFjPqNM068DDZK7yQHcBZ5O5TvRFG+xNsjzJ5nsuLRro4YJ1loI1mux7sjwEHIoUyNnl7R8cqJgDFXOgYg5UzIGKOVCxoeDvbxTMzRH72Am9V3IuAS+DNaoQuR5RKBpo0Wfwc/02AezfGgMvussXbaHWJRpo064SNY7iGPqKvGNo9y6+BLwN1qhCC80hrLRj9F6EPZpRZ45mXWD+q+/359MmMQcq5kDFHKiYAxVzoGIOVMyBijlQMQcq5kDFHKiYAxVzoGIOVMyBijlQMQcq5kDFHKiYAxVzoGIOVMyBijlQMQcq5kDFHKhYNNDFgrlRRSMNcbBg7mukQDTQFeBTMpd+97OMpt64O5ksfyB4q2bOLp9+wfQeMBys0cQbd4eBO8lcO1okJ9CFZHkEeELshv+mbaEt4Cm9j0eajxbK2VJGgdfAvmR+EB+RsRc4Atyi9xEZi8Ap4Md2NDIF/Kb+h61UNX4B52VplXRD0HhTx7Qwp5AJNg/cdQegGu2t91S7GeoPo98xowhCffoyBhwAdovrVmWFzcdifKm7ETMzMzMzM7OS/gBhx/vb3xaUFQAAAABJRU5ErkJggg=="/>
            </defs>
          </svg>                     
        </h2>
      </div>
    </div>
    <div class="foda-manager" v-if="!showGraph">
        <table>
            <tr>
                <td style="border:0"></td>
                <td style="border:0; border-right: 1px solid"></td>
                <th :colspan="values.o" style="background-color: #9DBFE5;"> 
                  <div class="table-head">
                    <button class="remove-button" @click="subtractO" v-if="values.o>1">
                      <img src="/rmbtn.png" alt="removearea" height="15" width="15">
                    </button>
                    <h3 v-if="values.o<=2">O</h3>
                    <h3 v-else-if="values.o<=4">Opo.</h3>
                    <h3 v-else>Oportunidades</h3>
                    <button class="add-button" @click="addO">
                      <img src="/addbtn.png" alt="addarea" width="15" height="15">
                    </button>
                  </div>
                </th>
                <th :colspan="values.a" style="background-color: #E46F6C;"> 
                  <div class="table-head">
                    <button class="remove-button" @click="subtractA" v-if="values.a>1">
                      <img src="/rmbtn.png" alt="removearea" height="15" width="15">
                    </button>
                    <h3 v-if="values.a<=2">A</h3>
                    <h3 v-else-if="values.a<=4">Ame.</h3>
                    <h3 v-else>Amenazas</h3>
                    <button class="add-button" @click="addA">
                      <img src="/addbtn.png" alt="addarea" width="15" height="15">
                    </button>
                  </div>
                </th>
                <th rowspan="2" style="background-color: #867A7A;">Total</th>
            </tr>
            <tr>
                <td style="border:0; border-bottom: 1px solid"></td>
                <td style="border:0; border-right: 1px solid; border-bottom: 1px solid"></td>
                <th v-for="col in values.o" :key="col" style="background-color:#D9D9D9">{{ "O"+col }}</th>
                <th v-for="col in values.a" :key="col" style="background-color:#D9D9D9">{{ "A"+col }}</th>
            </tr>
            <tr v-for="(row, rowIndex) in values.f" :key="rowIndex">
                <th v-if="row==1" :rowspan="values.f" style="background-color: #C6E5B1;">
                  <div class="table-side" style="transform: rotate(-90deg);">
                    <button class="add-button" @click="addF">
                      <img src="/addbtn.png" alt="addarea" width="15" height="15" style="transform: rotate(90deg);">
                    </button>
                    <h3 v-if="values.f<=2">F</h3>
                    <h3 v-else-if="values.f<=4">For.</h3>
                    <h3 v-else>Fortalezas</h3>
                    <button class="remove-button" @click="subtractF" v-if="values.f>1">
                      <img src="/rmbtn.png" alt="removearea" height="15" width="15" style="transform: rotate(90deg);">
                    </button>
                  </div>
                </th>
                <th style="background-color:#D9D9D9">{{ "F"+row }}</th>
                <td v-for="(col, colIndex) in values.o" :key="colIndex">
                    <dropdown :options="dropdownOptions" :matrixValue="values.matriz[rowIndex][colIndex]" @option-selected="onOptionSelected(rowIndex, colIndex, $event)" />
                </td>
                <td v-for="(col, colIndex) in values.a" :key="colIndex">
                    <dropdown :options="dropdownOptions" :matrixValue="values.matriz[rowIndex][colIndex+values.o]" @option-selected="onOptionSelected(rowIndex, colIndex+values.o, $event)" />
                </td>
                <td>{{ rowTotal(rowIndex) }}</td>
            </tr>
            <tr v-for="(row, rowIndex) in values.d" :key="rowIndex">
              <th v-if="row==1" :rowspan="values.d" style="background-color: #F9F9B1;">
                <div class="table-side" style="transform: rotate(-90deg);">
                  <button class="add-button" @click="addD">
                    <img src="/addbtn.png" alt="addarea" width="15" height="15" style="transform: rotate(90deg);">
                  </button>
                  <h3 v-if="values.d<=2">D</h3>
                  <h3 v-else-if="values.d<=4">Deb.</h3>
                  <h3 v-else>Debilidades</h3>
                  <button class="remove-button" @click="subtractD" v-if="values.d>1">
                    <img src="/rmbtn.png" alt="removearea" height="15" width="15" style="transform: rotate(90deg);">
                  </button>
                </div>
              </th>
                <th style="background-color:#D9D9D9">{{ "D"+row }}</th>
                <td v-for="(col, colIndex) in values.o" :key="colIndex">
                    <dropdown :options="dropdownOptions" :matrixValue="values.matriz[rowIndex+values.f][colIndex]" @option-selected="onOptionSelected(rowIndex+values.f, colIndex, $event)" />
                </td>
                <td v-for="(col, colIndex) in values.a" :key="colIndex">
                    <dropdown :options="dropdownOptions" :matrixValue="values.matriz[rowIndex+values.f][colIndex+values.o]" @option-selected="onOptionSelected(rowIndex+values.f, colIndex+values.o, $event)" />
                </td>
                <td>{{ rowTotal(rowIndex+values.f) }}</td>
            </tr>
            <tr>
                <th colspan="2" style="background-color:#867A7A">Total</th>
                <td v-for="(col, colIndex) in values.o" :key="colIndex">{{ colTotal(colIndex) }}</td>
                <td v-for="(col, colIndex) in values.a" :key="colIndex">{{ colTotal(colIndex+values.o) }}</td>
                <td>{{ total }}</td>
            </tr>
        </table>
    </div>
    <!--Este es el div de los gráficos-->
    <div class="foda-manager" v-if="showGraph">
      <p>
        Total Fuerzas: {{ values.totalf }} <br>
        Total Debilidades: {{ values.totald }} <br>
        Total Oportunidades: {{ values.totalo }} <br>
        Total Amenazas: {{ values.totala }} <br> 
      </p>
      <!--aca deben ir los graficos de torta-->
      <area-graph :totalf="totals.f" :totalo="totals.o" :totald="totals.d" :totala="totals.a"/>
    </div>
  </div>
    <div v-if="showModal">
      <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-container">
              <div class="modal-header">
                <slot name="header">
                  ¿Está seguro de eliminar el Área {{ this.delArea }}?
                </slot>
              </div>
              <div class="modal-footer">
                <slot name="footer">
                  <button class="modal-confirm-button" @click="confirmDelete">
                    Si
                  </button>
                  <button class="modal-cancel-button" @click="closeModal">
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
import '@fontsource/poppins';
import Dropdown from './Dropdown.vue';
import BarGraph from './BarGraph.vue'
import AreaGraph from './AreaGraph.vue'
  export default {
    components: { Dropdown, BarGraph, AreaGraph },
    name: "AreaManager",
    watch: {
        selected: function(newValue){
            this.values = this.foda.areas[newValue-1]
        },
        total: function(){
          this.totals={
            f:[],
            o:[],
            d:[],
            a:[]
          }
          for (let i=0; i<this.values.f; i++){
            this.totals.f.push(this.rowTotal(i))
          }
          for (let i=this.values.f; i<(this.values.f+this.values.d); i++){
            this.totals.d.push(this.rowTotal(i))
          }
          for (let j=0; j<this.values.o; j++){
            this.totals.o.push(this.colTotal(j))
          }
          for (let j=this.values.o; j<(this.values.o+this.values.a); j++){
            this.totals.a.push(this.colTotal(j))
          }
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
                   [0, 0]],
          totalf: 0,
          totalo: 0,
          totald: 0,
          totala: 0
        }] },
        values: {
            f: 1,
            o: 1,
            d: 1,
            a: 1,
            matriz: [[0, 0],
                     [0, 0]],
            totalf: 0,
            totalo: 0,
            totald: 0,
            totala: 0
        },
        showModal: false,
        delArea: 0,
        selectedOption: 0,
        dropdownOptions: [0, 2, 4, 6, 8, 10],
        showGraph: false,
        totals:{
          f:[],
          o:[],
          d:[],
          a:[]
        }
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
      },

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
      fTotal(){
        this.values.totalf=0
        for (let i=0;i<this.values.f;i++){
          this.values.totalf+=this.rowTotal(i)
        }
      },
      dTotal(){
        this.values.totald=0
        for (let i=this.values.f;i<(this.values.f+this.values.d);i++){
          this.values.totald+=this.rowTotal(i)
        }
      },
      oTotal(){
        this.values.totalo=0
        for (let i=0;i<this.values.o;i++){
          this.values.totalo+=this.colTotal(i)
        }
      },
      aTotal(){
        this.values.totala=0
        for (let i=this.values.o;i<(this.values.o+this.values.a);i++){
          this.values.totala+=this.colTotal(i)
        }
      },
      updateTotals(){
        this.fTotal()
        this.oTotal()
        this.dTotal()
        this.aTotal()
      },
      addArea() {
        this.foda.areas.push({
          f:1,
          o:1,
          d:1,
          a:1,
          matriz: [[0, 0],
                   [0, 0]],
          totalf: 0,
          totalo: 0,
          totald: 0,
          totala: 0                                                                                                                                       
        })
        this.count = this.foda.areas.length
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      removeArea(area) {
        this.showModal = true
        this.delArea = area
      },
      showArea(area){
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
      updateFoda(){
        this.foda = JSON.parse(localStorage.getItem("FODA"))
        this.foda.areas[this.selected-1] = this.values
        localStorage.setItem("FODA", JSON.stringify(this.foda))
      },
      addF() {
        this.values.f++;
        this.values.matriz.splice(this.values.f-1,0,new Array(this.values.o+this.values.a).fill(0))
        this.updateFoda()
      },
      subtractF() {
        this.values.f--;
        this.values.matriz.splice(this.values.f,1)
        this.updateTotals()
        this.updateFoda()
      },
      addO() {
        this.values.o++;
        for(let i=0;i<this.values.matriz.length;i++) this.values.matriz[i].splice(this.values.o-1,0,0)
        this.updateFoda()
      },
      subtractO() {
        this.values.o--;
        for(let i=0;i<this.values.matriz.length;i++) this.values.matriz[i].splice(this.values.o,1)
        this.updateTotals()
        this.updateFoda()
      },
      addD() {
        this.values.d++;
        this.values.matriz.push(new Array(this.values.o+this.values.a).fill(0))
        this.updateFoda()
      },
      subtractD() {
        this.values.d--;
        this.values.matriz.pop()
        this.updateTotals()
        this.updateFoda()
      },
      addA() {
        this.values.a++;
        for(let i=0;i<this.values.matriz.length;i++){
          this.values.matriz[i].push(0)
        }
        this.updateFoda()
      },
      subtractA() {
        this.values.a--;
        for(let i=0;i<this.values.matriz.length;i++){
          this.values.matriz[i].pop()
        }
        this.updateTotals()
        this.updateFoda()
      },
      onOptionSelected(row,col,option){
        this.values.matriz[row][col] = option
        this.updateTotals()
        this.updateFoda()
      }
    },
    mounted() {
      this.foda= { areas: [{
          f: 1,
          o: 1,
          d: 1,
          a: 1,
          matriz: [[0, 0],
                   [0, 0]],
          totalf: 0,
          totalo: 0,
          totald: 0,
          totala: 0
        }] }
      if (localStorage.getItem("FODA")!==null){
        this.foda = JSON.parse(localStorage.getItem("FODA"))
      }
      this.count = this.foda.areas.length
      this.selected = 1
      this.values = this.foda.areas[this.selected-1]
      localStorage.setItem("FODA", JSON.stringify(this.foda))
    }
  };
  </script>
  
<style scoped>
.main-div {
  display: grid;
  grid-template-columns: 1fr 15fr;
  grid-template-rows: 1fr 15fr;
  grid-column-gap: 20px;
}

.logo{
  grid-area: 1 / 1 / 2 / 2;
  place-self: center;
}
.logo-text{
  font-family: 'Poppins', sans-serif;
  font-weight: normal;
  font-style: normal;
  font-display: swap;
  font-size: 22px;
  color: #FFFFFF;
}
.area-manager{
  grid-area: 2 / 1 / 3 / 2;
  min-width: auto;
  max-width: fit-content;
  height: 90vh;
  margin: 2%;
}
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

  
.area-info{
  grid-area: 1 / 2 / 2 / 3;
  background-color: #FFFFFF;
  padding: 2%;
}
h1{
  font-family: 'Poppins', sans-serif;
  font-weight: 3000;
  font-size: 40px;
  color: #324855;
}
h2{
  font-family: 'Poppins', sans-serif;
  font-weight: normal;
  font-style: normal;
  font-display: swap;
  font-size: 20px;
  text-align: left;
}
svg{
  vertical-align: middle;
}
.area-name{
  margin-top:5px;
  border-style: solid;
  border-width: medium;
  border-color: #000000;
  border-radius: 15px;
  background-color: #F2E4AB;
  padding: 0 50px 0 50px;
  width: max-content;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.area-selection{
  padding: 0 5px 0 50px;
}
.total-area{
  margin-top:5px;
  border-style: solid;
  border-width: medium;
  border-color: #000000;
  border-radius: 15px;
  padding: 0 50px 0 50px;
  align-items: center;
  justify-content: space-between;
  background-color: #867A7A;
  width: max-content;
}
.selected-area{
  background-color: #DF4E4E;
}
.remove-area{
  justify-self: end;
  margin-left: 20px;
}
.remove-button{
  border:none;
  background-color:transparent;
  outline:none;
}
.add-area{
  padding: 0 5px 0 5px;
}
.add-button{
  border:none;
  background-color:transparent;
  outline:none;
}
.foda-manager{
  grid-area: 2 / 2 / 3 / 3;
  background: #FFFFFF;
  padding: 3%;
}
  table, th, td {
    border:1px solid black;
    border-spacing: 0;
    font-family: 'Poppins', sans-serif;
    font-weight: normal;
    font-style: normal;
    font-display: swap;
    font-size: 20px;
    text-align: center;
    padding: 10px;
    margin: 0;
  }
  table {
    border:0;
  }
  .table-head{
    align-items: center;
    height: 30px;
    font-weight: bold;
    display: flex;
    justify-content: space-between;
  }
  .table-side{
    align-items: center;
    font-weight: bold;
    display: flex;
    justify-content: space-around;
    width: 30px;
    align-content: center;
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
    border: 4px solid;
    padding: 7%;
    background-color: #fff;
    border-radius: 20px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
    transition: all 0.3s ease;
    font-family: 'Poppins', sans-serif;
    font-size: 25px;
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
  .modal-confirm-button{
    float: left;
    border: 3px solid;
    border-radius: 10px;
    font-family: 'Poppins', sans-serif;
    padding: 2% 11% 2% 11%;
    font-size: 20px;   
    margin-top: 2%;
    background-color:#DF4E4E;
  }
  .modal-cancel-button{
    float: right;
    border: 3px solid;
    border-radius: 10px;
    font-family: 'Poppins', sans-serif;
    padding: 2% 11% 2% 11%;
    font-size: 20px;   
    margin-top: 2%;
    background-color: #F2E4AB;
  }
  
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