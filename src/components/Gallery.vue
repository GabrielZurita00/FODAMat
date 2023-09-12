<template>
  
    <div>
        <a style="color:#000000;" href="/">
            <div class="area-name add-area">
              <svg fill="#000000" height="100" width="20" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 26.676 26.676" xml:space="preserve"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <g> <path d="M26.105,21.891c-0.229,0-0.439-0.131-0.529-0.346l0,0c-0.066-0.156-1.716-3.857-7.885-4.59 c-1.285-0.156-2.824-0.236-4.693-0.25v4.613c0,0.213-0.115,0.406-0.304,0.508c-0.188,0.098-0.413,0.084-0.588-0.033L0.254,13.815 C0.094,13.708,0,13.528,0,13.339c0-0.191,0.094-0.365,0.254-0.477l11.857-7.979c0.175-0.121,0.398-0.129,0.588-0.029 c0.19,0.102,0.303,0.295,0.303,0.502v4.293c2.578,0.336,13.674,2.33,13.674,11.674c0,0.271-0.191,0.508-0.459,0.562 C26.18,21.891,26.141,21.891,26.105,21.891z"></path> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> <g> </g> </g> </g></svg>
              Volver al inicio
            </div>
          </a>
          <br>
      <div class="album-container fondo">
        <div v-for="(album, index) in albums" :key="index" @click="showAlbum(index)" class="album">
          <img :src="'/images/' + album.cover" alt="Album Cover" class="album-cover">
          <h2 class="album-title">{{ album.title }}</h2>
        </div>
      </div>
      <div v-if="selectedAlbum !== null" class="slideshow">
        <h1>{{ selectedAlbum.title }}</h1>
        <img :src="'/images/' + selectedAlbum.images[currentImageIndex]" alt="Album Image" class="slideshow-image">
        <br>
        <button @click="prevImage" :disabled="currentImageIndex === 0" class="slideshow-button">Anterior</button>
        <button @click="nextImage" :disabled="currentImageIndex === selectedAlbum.images.length - 1" class="slideshow-button">Siguiente</button>
    </div>

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
            images: ["1.jpg", "2.jpg", "iz1.jpg", "iz2.jpg", "iz3.jpg", "iz4.jpg",]
          },
          {
            title: "Sobre la Tabla",
            cover: "tabla.png",
            images: ["t1.jpg", "t2.jpg", "t3.jpg"]
          },
          {
            title: "Gráficos de áreas",
            cover: "graph.png",
            images: ["g1.jpg", "g2.jpg"]
          },
          {
            title: "Análisis Comparativo ",
            cover: "radar.jpg",
            images: ["res1.jpg", "res2.jpg"]
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
    width: 200px;
    height: 200px;
    border: 1px solid #022C55;
    border-radius: 10px;
    transition: transform 0.2s;
    background: #fff;
  }
  
  .album-title {
    margin-top: 10px;
  }
  
  .album:hover .album-cover {
    transform: scale(1.05);
        background-color: #022C55;

  }
  
  .slideshow {
    text-align: center;
    color: #000000;
    margin-top: 20px;
  }
  
  .slideshow-image {
   
   
    border: 2px solid #022C55;
    border-radius: 10px;
  }
  
  .slideshow-button {
    margin-top: 10px;
    padding: 5px 15px;
    font-size: 16px;
    background-color: #DF4E4E;
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
    border-color: #000000;
    border-radius: 10px;
    background-color: #F2E4AB;
    padding: 0 50px 0 50px;
    width: max-content;
    height: 35px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    user-select: none;
  }
  .fondo{
		background-color:  #022C55;
		padding: 1rem;
		text-align: center;
		align-items: center;
		align-self: center;
		border-radius: 5px;
    color: #ffffff;
	}
  </style>
  