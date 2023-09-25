<template>
  <div>
    <div class="main-div">
      <div class="logo">
        <img src="/512.png" alt="Logo DUEA" width="120" height="120">
        <a style="color:#000000;" href="/">
          <div class="area-name add-area">
            <svg fill="#000000" height="20" width="20" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 26.676 26.676" xml:space="preserve"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <g> <path d="M26.105,21.891c-0.229,0-0.439-0.131-0.529-0.346l0,0c-0.066-0.156-1.716-3.857-7.885-4.59 c-1.285-0.156-2.824-0.236-4.693-0.25v4.613c0,0.213-0.115,0.406-0.304,0.508c-0.188,0.098-0.413,0.084-0.588-0.033L0.254,13.815 C0.094,13.708,0,13.528,0,13.339c0-0.191,0.094-0.365,0.254-0.477l11.857-7.979c0.175-0.121,0.398-0.129,0.588-0.029 c0.19,0.102,0.303,0.295,0.303,0.502v4.293c2.578,0.336,13.674,2.33,13.674,11.674c0,0.271-0.191,0.508-0.459,0.562 C26.18,21.891,26.141,21.891,26.105,21.891z"></path> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> </g> </g></svg>
            Volver al inicio
          </div>
        </a>
        <p class="logo-text">FODA Matemático</p>
      </div>
      <div class="area-manager">
        <div class="vertical-menu" v-if="count>=0">
          <div class="area-name">
            <h2>Áreas</h2>
          </div>
          <div class="area-name area-selection" v-for="area in count" :key="area" @click="showArea(area)" v-bind:class="((area!=selected)&&(!totalArea))?'unselected-area':'selected-area'" >
            <h2>Área {{ area }}

            </h2>
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
  
  
  
        <div class="totalArea" @click="totalArea=!totalArea" v-bind:class="totalArea ? 'total-selected':''">
          <button class="add-button">
          <h2>RESULTADO <br> COMPARATIVO</h2>
        </button>
        </div>
  
  
  
      </div>
      <div class="area-info">
        <h1>Tabla FODA</h1>
        <div v-if="!totalArea" class="area-title">
          <h2 v-if="selected>0">{{ "Área "+selected }}</h2>          
        </div>
        <div v-else class="totalArea">
          <h2>{{ "RESULTADO COMPARATIVO" }}</h2>
        </div>
        <div v-if="totalArea">
          <a href="/pdf">
              <h5 style="color:#000000;" href="/crear">
              <div class="selected-area area-name add-area">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <path d="M17 17H17.01M17.4 14H18C18.9319 14 19.3978 14 19.7654 14.1522C20.2554 14.3552 20.6448 14.7446 20.8478 15.2346C21 15.6022 21 16.0681 21 17C21 17.9319 21 18.3978 20.8478 18.7654C20.6448 19.2554 20.2554 19.6448 19.7654 19.8478C19.3978 20 18.9319 20 18 20H6C5.06812 20 4.60218 20 4.23463 19.8478C3.74458 19.6448 3.35523 19.2554 3.15224 18.7654C3 18.3978 3 17.9319 3 17C3 16.0681 3 15.6022 3.15224 15.2346C3.35523 14.7446 3.74458 14.3552 4.23463 14.1522C4.60218 14 5.06812 14 6 14H6.6M12 15V4M12 15L9 12M12 15L15 12" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path> </g></svg>
                Guardar FODA
              </div>
            </h5>
          </a>
        </div>
      </div>
  
  
     
  

        <div class="foda-manager" v-if="!totalArea">

          <table>
            <tr>
              <td style="border:0"></td>
              <td style="border:0"></td>
                  <td style="border:0; border-right: 1px solid"></td>
                  <th :colspan="values.o+values.a+1"> <h3>ÁMBITO EXTERNO</h3></th>
            </tr>
              <tr>
                  <td style="border:0"></td>
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
                <td style="border:0"></td>
                  <td style="border:0; border-bottom: 1px solid"></td>
                  
                  <td style="border:0; border-right: 1px solid; border-bottom: 1px solid"></td>
                 
                  <th v-for="col in values.o" :key="col" style="background-color:#D9D9D9">{{ "O"+col }}<span contenteditable="true"/></th>
                  <th v-for="col in values.a" :key="col" style="background-color:#D9D9D9">{{ "A"+col }}<span contenteditable="true"/></th>
              </tr>
              <tr>
                <th :rowspan="values.f+values.d+2" width="15"><h3 style="transform: rotate(-90deg); text-wrap: wrap;">ÁMBITO INTERNO</h3></th>
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
                  <th style="background-color:#D9D9D9">{{ "F"+row }}
                    <div>
                    </div>
                  </th>
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
                  <th style="background-color:#D9D9D9">{{ "D"+row }}
                    <div>
                    </div>
                  </th>
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

          <h1 class="graph-title">Análisis FODA</h1>
          <div class="area-graph">
            <div class="pie-graph">
              <pie-graph :fTotal="values.totalf" :oTotal="values.totalo" :dTotal="values.totald" :aTotal="values.totala" :graphW="400" :graphH="300"/>
            </div>
            <div class="bar-graph">
              <bar-graph :totalf="totals.f" :totalo="totals.o" :totald="totals.d" :totala="totals.a"/>
            </div>
          </div>
        
  
        </div>
  
  
  
      <!-- Total -->
      <div class="foda-manager" v-if="totalArea">
  
      <table>
        <thead>
          <tr>
            <th class="area-header area-1">Área</th>
            <th class="totalf-header">Total F</th>
            <th class="totald-header">Total D</th>
            <th class="totala-header">Total O</th>
            <th class="totalo-header">Total A</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(area, index) in foda.areas" :key="index">
            <td>Área {{ index + 1 }}</td>
            <td>{{ FPercent(area) }}%</td>
            <td>{{ DPercent(area) }}%</td>
            <td>{{ OPercent(area) }}%</td>
            <td>{{ APercent(area) }}%</td>
          </tr>
        </tbody>
      </table>
      <h1 class="graph-title">Análisis FODA</h1>
      <div class="bar-graph">
            <radar-chart :areas="foda.areas"/>
      </div>
    </div>
    </div>
    <div>
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
      
  </template>
    
  <script>
  import '@fontsource/poppins';
  import Dropdown from './Dropdown.vue';
  import PieGraph from './PieGraph.vue';
  import BarGraph from './BarGraph.vue';
  import RadarChart from './RadarChart.vue';
    export default {
      components: { Dropdown, PieGraph, BarGraph, RadarChart },
      name: "AreaManager",
      watch: {
          selected: function(newValue){
              this.values = this.foda.areas[newValue-1]
          },
          values: {
            handler(){
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
            },
            deep: true
          },
          totalArea: function(newValue){
            if (newValue){
              this.selected=1
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
          totalArea: false,
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
          this.totalArea=false
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
          console.log(option);
          this.updateTotals()
          this.updateFoda()
        },
        FPercent(area){
          return (area.totalf+area.totald+area.totalo+area.totala)!=0 ? Math.round((((area.totalf*100)/(area.totalf+area.totald+area.totalo+area.totala))+Number.EPSILON)*100)/100 : 0
        },
        DPercent(area){
          return (area.totalf+area.totald+area.totalo+area.totala)!=0 ? Math.round((((area.totald*100)/(area.totalf+area.totald+area.totalo+area.totala))+Number.EPSILON)*100)/100 : 0
        },
        OPercent(area){
          return (area.totalf+area.totald+area.totalo+area.totala)!=0 ? Math.round((((area.totalo*100)/(area.totalf+area.totald+area.totalo+area.totala))+Number.EPSILON)*100)/100 : 0
        },
        APercent(area){
          return (area.totalf+area.totald+area.totalo+area.totala)!=0 ? Math.round((((area.totala*100)/(area.totalf+area.totald+area.totalo+area.totala))+Number.EPSILON)*100)/100 : 0
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
      },
  
  
  
  
  
      created() {
      const storedData = localStorage.getItem('yourKey');
      if (storedData) {
        const parsedData = JSON.parse(storedData);
        this.foda = parsedData.foda;
        this.values = parsedData.values;
      }
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
    margin: 2%;
  }
  .vertical-menu {
    width: 200px; 
    align-content: left;
  }
  
  .graph-title{
    text-align: center;
  }
  
    
  .area-info{
    grid-area: 1 / 2 / 2 / 3;
    background-color: #FFFFFF;
    padding: 2%;
    border: 20px;
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
  a{
    text-decoration:none;
    color: #000000;
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
    user-select: none;
  }
  .area-title{
    margin-top:5px;
    border-style: solid;
    border-width: medium;
    border-color: #000000;
    border-radius: 15px;
    background-color: #DF4E4E;
    padding: 0 50px 0 50px;
    width: max-content;
    display: flex;
    align-items: center;
    justify-content: space-between;
    user-select: none;
  }
  .area-title h2{
    color: #fff;
    font-weight: bold;
  }
  .area-selection{
    padding: 0 5px 0 50px;
  }
  .unselected-area{
    background-color: #F2E4AB;
  }
  .selected-area{
    background-color: #DF4E4E;

  }
  .selected-area h2{
    color: #fff;
    font-weight: bold;
    
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
    padding: 0%;
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
  
  
    .totalArea{
      margin-top:5px;
      border-style: solid;
      border-width: medium;
      border-color: #000000;
      border-radius: 15px;
      padding: 0 20px 0 20px;
      align-items: center;
      justify-content: space-between;
      background-color: #867A7A;
      width: max-content;
    }
   .total-selected{
    margin-top:5px;
      border-style: solid;
      border-width: medium;
      border-color: #000000;
      border-radius: 15px;
      padding: 0 20px 0 20px;
      align-items: center;
      justify-content: space-between;
      background-color: #474141;
      width: max-content;
   }
   .total-selected h2{
    font-weight: bold;
    color:#fff;
   }
  
  
  
  /* Colores de las celdas de la tabla de totales*/
  .area-header {
    background-color: #eee; /* Yellow */
  }
  
  .totala-header {
    background-color: #E46F6C; /* Green */
  }
  
  .totald-header {
    background-color: #F9F9B1; /* Blue */
  }
  
  .totalf-header {
    background-color: #C6E5B1; /* Orange */
  }
  
  .totalo-header {
    background-color: #9DBFE5; /* Red */
  }

  .bar-graph{
    width: 50vw;
    margin: auto;
  }
  .pie-graph{
    width: 70vw;
    margin: auto;
  }
  .area-graph{
    display: flex;
    flex-direction: column;
    flex-flow: space-between;
    gap: 10px;
  }
  span 
{
    size: 14px;
}
  </style>