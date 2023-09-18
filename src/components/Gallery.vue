<template>
  <div class="container">
    <div class="box">
        <div class="box-row">
    <div>
        <a style="color:#000000; size: 18px" href="/">
            <div class="area-name add-area">
              Volver al inicio
            </div>
          </a>
          <div class="box-cell box1">
      <div class="album-container fondo">
        <div v-for="(album, index) in albums" :key="index" @click="showAlbum(index)" class="album">
          <img :src="'/images/' + album.cover" alt="Album Cover" class="album-cover">
          <h2 style="font-size: 19px;"  class="album-title">{{ album.title }}</h2>
        </div>
      </div></div>
      <div class="box-cell box1">
      <div v-if="selectedAlbum !== null" class="slideshow">
        <h1>{{ selectedAlbum.title }}</h1>
        <button @click="prevImage" :disabled="currentImageIndex === 0" class="slideshow-button">Anterior</button>
        <button @click="nextImage" :disabled="currentImageIndex === selectedAlbum.images.length - 1" class="slideshow-button">Siguiente</button>
    
        <img :src="'/images/' + selectedAlbum.images[currentImageIndex]" alt="Album Image" class="slideshow-image">
        <br>
        <button @click="prevImage" :disabled="currentImageIndex === 0" class="slideshow-button">Anterior</button>
        <button @click="nextImage" :disabled="currentImageIndex === selectedAlbum.images.length - 1" class="slideshow-button">Siguiente</button>
    </div>
  </div></div></div></div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        albums: [
          {
            title: "Introducción",
            cover: "intro.png",
            images: ["tut.png","b1.png","i1.png","i2.png","i3.png","i4.png"]
          },
          {
            title: "Sobre la Tabla",
            cover: "tabla.png",
            images: ["t1.png", "t2.png", "t3.png"]
          },
          {
            title: "Gráficos de áreas",
            cover: "graph.png",
            images: ["g1.png", "g2.png"]
          },
          {
            title: "Análisis Comparativo ",
            cover: "radar.jpg",
            images: ["r1.png", "r2.png", "r3.png"]
          },
        ],
        selectedAlbum: null,
        currentImageIndex: 0
      };
    },
    methods: {
      showAlbum(index) {
        this.selectedAlbum = this.albums[index];
        this.currentImageIndex = 0;
      },
      prevImage() {
        if (this.currentImageIndex > 0) {
          this.currentImageIndex--;
        }
      },
      nextImage() {
        if (this.currentImageIndex < this.selectedAlbum.images.length - 1) {
          this.currentImageIndex++;
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .album-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 15px;

  }
  .album-bg {
    background-color: #022C55;

  }
  
  .album {
    cursor: pointer;
    text-align: center;
  }
  
  .album-cover {
    width: 130px;
    height: 130px;
    border: 1px solid #022C55;
    border-radius: 10px;
    transition: transform 0.2s;
    background: #fff;
  }
  
  .album-title {
    margin-top: 5px;
  }
  
  .album:hover .album-cover {
    transform: scale(1.05);
        background-color: #ffffff;

  }
  
  .slideshow {
    text-align: center;
    color: #ffff;
    margin-top: 2px;
  vertical-align: top;
  }
  
  .slideshow-image {
   
    border: 2px solid #022C55;
    border-radius: 10px;
    position: relative;
    vertical-align: top;
    width: 700px;
    height: 570px;
  }
  
  .slideshow-button {
    margin-top: 10px;
    padding: 5px 15px;
    font-size: 16px;
    background-color: #DF4E4E;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .slideshow-button:disabled {
    background-color: #aaa;
    cursor: not-allowed;
  }
  .add-area{
    padding: 0 5px 0 5px;
  }
  .area-name{
    margin-top:5px;
    border-style: solid;
    border-width: medium;
    border-color: #aaa;
    border-radius: 10px;
    background-color: #F2E4AB;
    padding: 0 50px 0 50px;
    width: 110px;
    height: 28px;
    display: flex;
    justify-content: space-between;
    user-select: none;
  }
  .fondo{
		background-color:  #fff;
		padding: 1rem;
	
		border-radius: 5px;
    color: #aaa;
	}.container .box {
    width:200px;
    display:table;
    align-items: center;
}
.container .box .box-row {
    display:table-row;
    vertical-align: top;

}
.container .box .box-cell {
    display:table-cell;
    width:20%;
    padding:10px;
    margin-top: 250px;
    vertical-align: top;

}
  </style>
  