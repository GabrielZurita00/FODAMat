<template>
    <div >
        <div class="area-info">
            <div v-for="(area, index) in foda.areas" :key="index" :id="'area-'+(index+1)">
                
                <div class="foda-manager">
                  <div :id="'tabla-'+(index+1)" v-show="showGraphElement">
                    <table>
                      <tr>
                        <td style="border:0"></td>
                        <td style="border:0"></td>
                            <td style="border:0; border-right: 1px solid"></td>
                            <th :colspan="area.o+area.a+1"> <h3>ÁMBITO EXTERNO</h3></th>
                      </tr>
                        <tr>
                            <td style="border:0"></td>
                            <td style="border:0"></td>
                            <td style="border:0; border-right: 1px solid"></td>
                            <th :colspan="area.o" style="background-color: #9DBFE5;"> 
                              <div class="table-head">
                                <h3 v-if="area.o<=4">Opo.</h3>
                                <h3 v-else>Oportunidades</h3>
                              </div>
                            </th>
                            <th :colspan="area.a" style="background-color: #E46F6C;"> 
                              <div class="table-head">
                                <h3 v-if="area.a<=4">Ame.</h3>
                                <h3 v-else>Amenazas</h3>
                              </div>
                            </th>
                            <th rowspan="2" style="background-color: #867A7A;">Total</th>
                        </tr>
                        <tr>
                          <td style="border:0; border-bottom: 1px solid"></td>
                            <td style="border:0; border-bottom: 1px solid"></td>
                            
                            <td style="border:0; border-right: 1px solid; border-bottom: 1px solid"></td>
                           
                            <th v-for="col in area.o" :key="col" style="background-color:#D9D9D9">{{ "O"+col }}</th>
                            <th v-for="col in area.a" :key="col" style="background-color:#D9D9D9">{{ "A"+col }}</th>
                        </tr>
                        <tr>
                          <th :rowspan="area.f+area.d+2" width="15"><h3 style="transform: rotate(-90deg); text-wrap: wrap;">ÁMBITO INTERNO</h3></th>
                        </tr>
                        <tr v-for="(row, rowIndex) in area.f" :key="rowIndex">
                          
                            <th v-if="row==1" :rowspan="area.f" style="background-color: #C6E5B1;">
                              <div class="table-side" style="transform: rotate(-90deg);">
                                <h3 v-if="area.f<=4">For.</h3>
                                <h3 v-else>Fortalezas</h3>
                              </div>
                            </th>
                            <th style="background-color:#D9D9D9">{{ "F"+row }}</th>
                            <td v-for="(col, colIndex) in area.o" :key="colIndex">
                                {{ area.matriz[rowIndex][colIndex] }}
                            </td>
                            <td v-for="(col, colIndex) in area.a" :key="colIndex">
                                {{area.matriz[rowIndex][colIndex+area.o]}}
                            </td>
                            <td>{{ rowTotal(index,rowIndex) }}</td>
                        </tr>
                        <tr v-for="(row, rowIndex) in area.d" :key="rowIndex">
                          <th v-if="row==1" :rowspan="area.d" style="background-color: #F9F9B1;">
                            <div class="table-side" style="transform: rotate(-90deg);">
                              <h3 v-if="area.d<=4">Deb.</h3>
                              <h3 v-else>Debilidades</h3>
                            </div>
                          </th>
                            <th style="background-color:#D9D9D9">{{ "D"+row }}</th>
                            <td v-for="(col, colIndex) in area.o" :key="colIndex">
                                {{area.matriz[rowIndex+area.f][colIndex]}}
                            </td>
                            <td v-for="(col, colIndex) in area.a" :key="colIndex">
                                {{area.matriz[rowIndex+area.f][colIndex+area.o]}}
                            </td>
                            <td>{{ rowTotal(index,rowIndex+area.f) }}</td>
                        </tr>
                        <tr>
                            <th colspan="2" style="background-color:#867A7A">Total</th>
                            <td v-for="(col, colIndex) in area.o" :key="colIndex">{{ colTotal(index,colIndex) }}</td>
                            <td v-for="(col, colIndex) in area.a" :key="colIndex">{{ colTotal(index,colIndex+area.o) }}</td>
                            <td>{{ total(index) }}</td>
                        </tr>
                    </table>
                  </div>
                  <div class="pie-graph" :id="'pie-'+(index+1)" v-show="showGraphElement">
                    <pie-graph :fTotal="area.totalf" :oTotal="area.totalo" :dTotal="area.totald" :aTotal="area.totala" :graphW="250" :graphH="250"/>
                  </div>
                  <div class="bar-graph" :id="'bar-'+(index+1)" v-show="showGraphElement">
                    <bar-graph :totalf="totals[index].f" :totalo="totals[index].o" :totald="totals[index].d" :totala="totals[index].a"/>
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
                  <slot name="header" v-if="loading">
                    Cargando documento...
                  </slot>
                  <slot name="header" v-else>
                    Elija un formato
                  </slot>
                </div>
                <div class="modal-footer">
                  <slot name="footer" v-if="!loading">
                    <button class="modal-confirm-button" @click="print()">
                      PDF
                    </button>
                    <button class="modal-cancel-button" @click="word('fulltable', 'foda')">
                      WORD
                    </button>
                  </slot>
                </div>
              </div>
            </div>
          </div>
        </transition>
      </div>
      <div id="fulltable">
        <div v-for="(area, index) in foda.areas" :key="index">
                    <div :id="'tabla-img-'+(index+1)">
                      <div class="area-title">
                        <h2 >{{ "Área "+(index+1) }}</h2>
                      </div>
                      <h1>Tabla FODA</h1>
                    </div>
                    <div class="area-graph" :id="'grafico-'+(index+1)">
                      
                      
                      <div class="pie-graph" :id="'pie-img-'+(index+1)">
                        <div class="area-title">
                          <h2 >{{ "Área "+(index+1) }}</h2>
                        </div>
                          <h1 class="graph-title">Análisis FODA</h1>
                      </div>
                      
                      <div class="bar-graph" :id="'bar-img-'+(index+1)">
                        <div class="area-title">
                          <h2 >{{ "Área "+(index+1) }}</h2>
                        </div>
                        <h1 class="graph-title">Análisis FODA</h1>
                      </div>
                      
                    </div>
                  </div>
      </div>
    </div>
</template>

<script>
import jsPDF from 'jspdf';
import html2canvas from 'html2canvas';
import mammoth from 'mammoth';
import PieGraph from './PieGraph.vue';
import BarGraph from './BarGraph.vue';
import RadarChart from './RadarChart.vue';
export default {
  components: { jsPDF, html2canvas, mammoth, PieGraph, BarGraph, RadarChart },
    data(){
        return{
            count: 1,
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
            showGraphElement: true,
            totals: [{
              f:[],
              o:[],
              d:[],
              a:[]
            }],
            showModal: true,
            loading: true
        }
    },
    methods: {
        rowTotal(index,row){
          return this.foda.areas[index].matriz[row].reduce((acc, cell) => acc+cell, 0)
        },
        colTotal(index,col){
        const rows = this.foda.areas[index].matriz.length
          let sum = 0;
          for (let j=0;j<rows;j++){
            sum+=this.foda.areas[index].matriz[j][col]
          }
          return sum 
        }, 
        total(index){
          let sum = 0;
          const rows = this.foda.areas[index].matriz.length
          const cols = this.foda.areas[index].matriz[0].length
          for (let i=0;i<cols;i++){
              for(let j=0;j<rows;j++){
                  sum+=this.foda.areas[index].matriz[j][i]
              }
          }
          return sum
        },
        chart2img(index){
          const tablaCanvas = document.getElementById('tabla-'+index);
          html2canvas(tablaCanvas).then(canvas => {
            const chartImageBase64 = canvas.toDataURL('image/png')

            const img = document.createElement('img');
            img.src = chartImageBase64

            document.getElementById('tabla-img-'+index).appendChild(img);
          })
          const pieCanvas = document.getElementById('pie-'+index);
          html2canvas(pieCanvas).then(canvas => {
            const chartImageBase64 = canvas.toDataURL('image/png')

            const img = document.createElement('img');
            img.src = chartImageBase64

            document.getElementById('pie-img-'+index).appendChild(img);
          })
          const barCanvas = document.getElementById('bar-'+index);
          html2canvas(barCanvas).then(canvas => {
            const chartImageBase64 = canvas.toDataURL('image/png')

            const img = document.createElement('img');
            img.src = chartImageBase64

            document.getElementById('bar-img-'+index).appendChild(img);
          })
        },
        print(){

          const pdf = new jsPDF({ orientation: 'landscape' });


          const addPageToPdf = (element) => {
            return html2canvas(element).then(canvas => {
              const imgData = canvas.toDataURL('image/png');
              pdf.addPage();
              pdf.addImage(imgData, 'PNG', 10, 10);
            });
          };

          const sectionsToCapture = []
          for (let i=0;i<this.count;i++){
            sectionsToCapture.push(`tabla-img-${i+1}`)
            sectionsToCapture.push(`pie-img-${i+1}`)
            sectionsToCapture.push(`bar-img-${i+1}`)
          }

          const capturePromises = sectionsToCapture.map(sectionId => {
            const sectionElement = document.getElementById(sectionId);
            return addPageToPdf(sectionElement);
          });

          Promise.all(capturePromises).then(() => {
            pdf.save('foda.pdf');
          });
        },
        word(element, filename = ''){
          var preHtml = "<html xmlns:o='urn:schemas-microsoft-com:office:office' xmlns:w='urn:schemas-microsoft-com:office:word' xmlns='http://www.w3.org/TR/REC-html40'><head><meta charset='utf-8'><title>Export HTML To Doc</title></head><body>";
    var postHtml = "</body></html>";
    var html = preHtml+document.getElementById(element).innerHTML+postHtml;

    var blob = new Blob(['\ufeff', html], {
        type: 'application/msword'
    });
    
    // Specify link url
    var url = 'data:application/vnd.ms-word;charset=utf-8,' + encodeURIComponent(html);
    
    // Specify file name
    filename = filename?filename+'.doc':'document.doc';
    
    // Create download link element
    var downloadLink = document.createElement("a");

    document.body.appendChild(downloadLink);
    
    if(navigator.msSaveOrOpenBlob ){
        navigator.msSaveOrOpenBlob(blob, filename);
    }else{
        // Create a link to the file
        downloadLink.href = url;
        
        // Setting the file name
        downloadLink.download = filename;
        
        //triggering the function
        downloadLink.click();
    }
    
    document.body.removeChild(downloadLink);
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
        this.totals= []
        for (let ar=0;ar<this.count;ar++){
          this.totals.push({
              f:[],
              o:[],
              d:[],
              a:[]
            })
          for (let i=0; i<this.foda.areas[ar].f; i++){
                this.totals[ar].f.push(this.rowTotal(ar,i))
              }
              for (let i=this.foda.areas[ar].f; i<(this.foda.areas[ar].f+this.foda.areas[ar].d); i++){
                this.totals[ar].d.push(this.rowTotal(ar,i))
              }
              for (let j=0; j<this.foda.areas[ar].o; j++){
                this.totals[ar].o.push(this.colTotal(ar,j))
              }
              for (let j=this.foda.areas[ar].o; j<(this.foda.areas[ar].o+this.foda.areas[ar].a); j++){
                this.totals[ar].a.push(this.colTotal(ar,j))
              }
              
        }
        setTimeout(()=>{
          for (let i=1;i<=this.count;i++){
            this.chart2img(i)
          }
        },1555)
        setTimeout(()=>{
          this.showGraphElement=false
        },2525)
        setTimeout(()=>{
          this.loading=false
        },3333)
      },
}
</script>

<style>

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
    .bar-graph{
      width: 50vw;
      margin: auto;
    }
    .pie-graph{
      margin: 0vw;
      width: 50vw;
    }
    .area-graph{
      display: flex;
      flex-direction: column;
      flex-flow: space-between;
      gap: 10px;
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
      background-color:#b30c04;
    }
    .modal-cancel-button{
      float: right;
      border: 3px solid;
      border-radius: 10px;
      font-family: 'Poppins', sans-serif;
      padding: 2% 11% 2% 11%;
      font-size: 20px;   
      margin-top: 2%;
      background-color: #142c84;
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