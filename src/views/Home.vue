<template>
  <div class="home-container">
    <div class="contenido">
      <img src="/images/logo.png" alt="Logo Little Caesars" class="logo-img" />
      <img src="/images/titulohome.png" alt="Título del juego" class="titulo-img" />
      <h1 class="titulo">TIENES 30 SEGUNDOS</h1>
      <div class="insignia-contenedor">
        <img src="/images/insignia-home.png" alt="Insignia" class="insignia-img" />
        <div class="botones">
          <button class="btn btn-blanco" @click="iniciarJuego()">
              COMENZAR
          </button>
        </div>
      </div>      
    </div>   
  </div>
</template>

<script>

export default {
  components: {
  },
  data() {
    return {
    }
  },
  methods: {
    iniciarJuego() {
      this.mostrarPopup = false
      this.$router.push(`/juego`)     
    }
  }
}
</script>

<style scoped>
.home-container {
  background-image: url('/images/fondo.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.contenido {
  width: 100%;
  max-width: 45vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  height: 89vh;
  box-sizing: border-box;
  text-align: center;  
}
.contenido .titulo {
  color: #1A1413;
  font-family: 'EmigreTitulo', sans-serif !important;
  font-size: 4vh;
  margin-top: 1vh;
}

.titulo-img {
  width: 100%;
  margin-top: 3rem;
}

.insignia-contenedor {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 115%;
}

.insignia-img {
  margin-top: 1rem;
  width: 120%;
  display: block;
}


.btn {
  position: absolute;
  width: 55%;
  height: 9rem;
  bottom: 10%; /* ajusta esto según dónde lo quieras */
  left: 50%;
  transform: translateX(-50%);
  border: none;
  cursor: pointer;
  background-color: #EB6608;
  font-size: 5vh; /* más pequeño */
  color: #FFF;
  margin-bottom: 2vh;
  text-transform: uppercase;
  font-family: 'EmigreTitulo', sans-serif !important;
  border-radius: 15px;
}



/* Responsivo para celulares pequeños */
@media (max-width: 480px) {
  .contenido {
    height: 95vh;
    padding: 1rem 1rem;
  }

  .titulo-img {
    max-width: 70%;
    margin-bottom: 2rem;
    margin-top: 2rem;
  }

   .logo-img {
    max-width: 85%;
    margin-top: 2rem;
  }

   .insignia-contenedor {
    margin-top: 3.5rem;
    width: 110%;
  }

  .insignia-img {
    width: 110%;
    display: block;
  }

  .btn {
    width: 55%;
    height: 6rem;
    bottom: 10%; /* ajusta esto según dónde lo quieras */
    left: 50%;
    font-size: 4.5vh; /* más pequeño */
    margin-bottom: -2vh;
    border-radius: 15px;
  }

}
</style>
