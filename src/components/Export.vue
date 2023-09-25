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
                    <div id="tabla-total" v-show="showGraphElement">
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
                      
                    </div>
                    <div class="bar-graph" id="graph-total" v-show="showGraphElement">
                            <radar-total :areas="foda.areas"/>
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
                    Elija un formato para guardar
                  </slot>
                </div>
                <div class="modal-footer">
                  <slot name="footer" v-if="!loading">
                    <button class="modal-confirm-button" @click="print()">
                      <svg width="45" height="45" viewBox="0 0 512 512" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" fill="#000000"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <title>pdf-document</title> <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd"> <g id="add" fill="#000000" transform="translate(85.333333, 42.666667)"> <path d="M75.9466667,285.653333 C63.8764997,278.292415 49.6246897,275.351565 35.6266667,277.333333 L1.42108547e-14,277.333333 L1.42108547e-14,405.333333 L28.3733333,405.333333 L28.3733333,356.48 L40.5333333,356.48 C53.1304778,357.774244 65.7885986,354.68506 76.3733333,347.733333 C85.3576891,340.027178 90.3112817,328.626053 89.8133333,316.8 C90.4784904,304.790173 85.3164923,293.195531 75.9466667,285.653333 L75.9466667,285.653333 Z M53.12,332.373333 C47.7608867,334.732281 41.8687051,335.616108 36.0533333,334.933333 L27.7333333,334.933333 L27.7333333,298.666667 L36.0533333,298.666667 C42.094796,298.02451 48.1897668,299.213772 53.5466667,302.08 C58.5355805,305.554646 61.3626692,311.370371 61.0133333,317.44 C61.6596233,323.558965 58.5400493,329.460862 53.12,332.373333 L53.12,332.373333 Z M150.826667,277.333333 L115.413333,277.333333 L115.413333,405.333333 L149.333333,405.333333 C166.620091,407.02483 184.027709,403.691457 199.466667,395.733333 C216.454713,383.072462 225.530463,362.408923 223.36,341.333333 C224.631644,323.277677 218.198313,305.527884 205.653333,292.48 C190.157107,280.265923 170.395302,274.806436 150.826667,277.333333 L150.826667,277.333333 Z M178.986667,376.32 C170.098963,381.315719 159.922142,383.54422 149.76,382.72 L144.213333,382.72 L144.213333,299.946667 L149.333333,299.946667 C167.253333,299.946667 174.293333,301.653333 181.333333,308.053333 C189.877212,316.948755 194.28973,329.025119 193.493333,341.333333 C194.590843,354.653818 189.18793,367.684372 178.986667,376.32 L178.986667,376.32 Z M254.506667,405.333333 L283.306667,405.333333 L283.306667,351.786667 L341.333333,351.786667 L341.333333,329.173333 L283.306667,329.173333 L283.306667,299.946667 L341.333333,299.946667 L341.333333,277.333333 L254.506667,277.333333 L254.506667,405.333333 L254.506667,405.333333 Z M234.666667,7.10542736e-15 L9.52127266e-13,7.10542736e-15 L9.52127266e-13,234.666667 L42.6666667,234.666667 L42.6666667,192 L42.6666667,169.6 L42.6666667,42.6666667 L216.96,42.6666667 L298.666667,124.373333 L298.666667,169.6 L298.666667,192 L298.666667,234.666667 L341.333333,234.666667 L341.333333,106.666667 L234.666667,7.10542736e-15 L234.666667,7.10542736e-15 Z" id="document-pdf"> </path> </g> </g> </g></svg>
                      PDF
                    </button>
                    <button class="modal-cancel-button" @click="word('fulltable', 'foda')">
                      <svg width="40" height="40" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <path fill-rule="evenodd" clip-rule="evenodd" d="M10 1C9.73478 1 9.48043 1.10536 9.29289 1.29289L3.29289 7.29289C3.10536 7.48043 3 7.73478 3 8V20C3 21.6569 4.34315 23 6 23H10C10.5523 23 11 22.5523 11 22C11 21.4477 10.5523 21 10 21H6C5.44772 21 5 20.5523 5 20V9H10C10.5523 9 11 8.55228 11 8V3H18C18.5523 3 19 3.44772 19 4V9C19 9.55228 19.4477 10 20 10C20.5523 10 21 9.55228 21 9V4C21 2.34315 19.6569 1 18 1H10ZM9 7H6.41421L9 4.41421V7ZM12.952 12.694C12.783 12.1682 12.2198 11.879 11.694 12.048C11.1682 12.217 10.879 12.7802 11.048 13.306L13.298 20.306C13.4309 20.7196 13.8156 21 14.25 21C14.6844 21 15.0691 20.7196 15.202 20.306L16.5 16.2679L17.798 20.306C17.9309 20.7196 18.3156 21 18.75 21C19.1844 21 19.5691 20.7196 19.702 20.306L21.952 13.306C22.121 12.7802 21.8318 12.217 21.306 12.048C20.7802 11.879 20.217 12.1682 20.048 12.694L18.75 16.7321L17.452 12.694C17.3191 12.2804 16.9344 12 16.5 12C16.0656 12 15.6809 12.2804 15.548 12.694L14.25 16.7321L12.952 12.694Z" fill="#000000"></path> </g></svg>
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
                  <div class="foda-manager" id="total-img">
                    <div class="totalArea">
                      <h2>{{ "RESULTADO COMPARATIVO" }}</h2>
                    </div>
                    <h1>Tabla FODA</h1>
                  </div>
                  <div class="bar-graph" id="radar-img">
                    <h1 class="graph-title">Análisis FODA</h1>
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
import RadarTotal from './RadarTotal.vue';
export default {
  components: { jsPDF, html2canvas, mammoth, PieGraph, BarGraph, RadarTotal },
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
        total2img(){
          const totalCanvas = document.getElementById('tabla-total');
          html2canvas(totalCanvas).then(canvas => {
            const chartImageBase64 = canvas.toDataURL('image/png')

            const img = document.createElement('img');
            img.src = chartImageBase64

            document.getElementById('total-img').appendChild(img);
          })
          const radarCanvas = document.getElementById('graph-total');
          html2canvas(radarCanvas).then(canvas => {
            const chartImageBase64 = canvas.toDataURL('image/png')

            const img = document.createElement('img');
            img.src = chartImageBase64

            document.getElementById('radar-img').appendChild(img);
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
          sectionsToCapture.push('total-img')
          sectionsToCapture.push('radar-img')

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
          this.total2img()
        },2222)
        setTimeout(()=>{
          this.showGraphElement=false
        },2525)
        setTimeout(()=>{
          this.loading=false
        },3333)
      },
      created() {
      const storedData = localStorage.getItem('yourKey');
      if (storedData) {
        const parsedData = JSON.parse(storedData);
        this.foda = parsedData.foda;
        this.values = parsedData.values;
      }
    }
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
      padding: 2% 2% 2% 2%;
      font-size: 20px;   
      margin-top: 2%;
      background-color:#b30c04;
    }
    .modal-cancel-button{
      float: right;
      border: 3px solid;
      border-radius: 10px;
      font-family: 'Poppins', sans-serif;
      padding: 2% 2% 2% 2%;
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
    .modal-footer{
      display: flex;
      justify-content: space-around;
      width: 100%;
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

</style>