<template>
    <div>
        <div class="area-info">
            <div v-for="(area, index) in foda.areas" :key="index">
                <div class="area-title">
                <h2 >{{ "Área "+(index+1) }}</h2>
                </div>
                <h1>Tabla FODA</h1>
                <div class="foda-manager" v-if="!totalArea">
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
                                <h3 v-if="area.o<=2">O</h3>
                                <h3 v-else-if="area.o<=4">Opo.</h3>
                                <h3 v-else>Oportunidades</h3>
                              </div>
                            </th>
                            <th :colspan="area.a" style="background-color: #E46F6C;"> 
                              <div class="table-head">
                                <h3 v-if="area.a<=2">A</h3>
                                <h3 v-else-if="area.a<=4">Ame.</h3>
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
                                <h3 v-if="area.f<=2">F</h3>
                                <h3 v-else-if="area.f<=4">For.</h3>
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
                            <td>{{ rowTotal(rowIndex) }}</td>
                        </tr>
                        <tr v-for="(row, rowIndex) in area.d" :key="rowIndex">
                          <th v-if="row==1" :rowspan="area.d" style="background-color: #F9F9B1;">
                            <div class="table-side" style="transform: rotate(-90deg);">
                              <h3 v-if="area.d<=2">D</h3>
                              <h3 v-else-if="area.d<=4">Deb.</h3>
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
                            <td>{{ rowTotal(rowIndex+area.f) }}</td>
                        </tr>
                        <tr>
                            <th colspan="2" style="background-color:#867A7A">Total</th>
                            <td v-for="(col, colIndex) in area.o" :key="colIndex">{{ colTotal(colIndex) }}</td>
                            <td v-for="(col, colIndex) in area.a" :key="colIndex">{{ colTotal(colIndex+area.o) }}</td>
                            <td>{{ total }}</td>
                        </tr>
                    </table>
          
                    <h1 class="graph-title">Análisis FODA</h1>
                  
            
                  </div>
            </div>
        </div>

    </div>
</template>

<script>
import jsPDF from 'jspdf';
import html2canvas from 'html2canvas';
export default {
  components: { jsPDF, html2canvas },
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
            }] }
        }
    },
    methods: {
        rowTotal(index){
          //return this.values.matriz[index].reduce((acc, cell) => acc+cell, 0)
        },
        colTotal(index){
         /* const rows = this.values.matriz.length
          let sum = 0;
          for (let j=0;j<rows;j++){
            sum+=this.values.matriz[j][index]
          }
          return sum */
        }, 
        print(){
          const chartCanvas = document.getElementById('radarchart');
          html2canvas(chartCanvas).then(canvas => {
            const chartImageBase64 = canvas.toDataURL('image/png')

            const img = document.createElement('img');
            img.src = chartImageBase64

            document.getElementById('imgchart').appendChild(img);
          })
          

          const element = document.getElementById('fulltable'); // Replace 'divToExport' with the ID of the <div> you want to export.

          const pdf = new jsPDF();

          // Use html2canvas to capture the <div> as an image
          html2canvas(element).then(canvas => {
            const imgData = canvas.toDataURL('image/png');

            // Add the image to the PDF
            pdf.addImage(imgData, 'PNG', 0, 0);

            // Save or download the PDF
            pdf.save('exported.pdf');
          });

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
</style>