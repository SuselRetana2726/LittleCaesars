<template>
  <div class="juego-memoria">
    <img src="/images/mfondo.jpeg" alt="Fondo" class="fondo" />

    <div class="contenido">
      <img src="/images/c_slogan.png" alt="Slogan" class="cslogan-img" />

      <div class="temporizador">
        <span>00:{{ tiempoFormateado }}</span>
      </div>
      <div class="area-juego">
        <div class="tablero">
          <div v-for="(carta, index) in cartas" :key="index" class="carta" @click="voltearCarta(index)"
            :class="{ mezclando: mezclando }" :style="mezclando ? {
              '--dx': carta.dx + 'px',
              '--dy': carta.dy + 'px',
              '--rot': carta.rot + 'deg',
              animationDelay: (index * 0.06) + 's'
            } : {}">
            <div class="card-inner" :class="{ girada: carta.volteada || carta.encontrada }">
              <div class="card-front">
                <img src="/images/card.png" alt="Reverso" />
              </div>
              <div class="card-back">
                <img :src="getImagenCarta(carta.valor)" :alt="carta.valor" />
              </div>
            </div>
          </div>
        </div>

        <div class="acciones">
          <button @click="home" class="boton-icono boton-home">
            <img src="/icons/casa.png" alt="Inicio" />
          </button>
        </div>
      </div>
    </div>

    <PopUpGanaste
      :visible="ganaste" 
      @iniciar="aceptarGanaste"      
    />

    <PopUpPerdiste
      :juego="juegoSeleccionado" 
      :visible="perdiste" 
      :parejas="parejasEncontradas"
      @cerrar="salirPerdiste"
      @iniciar="reiniciarPerdiste"
    />
  </div>



</template>

<script>
import PopUpGanaste from '../components/PopUpWin.vue'
import PopUpPerdiste from '../components/PopUpLose.vue'

export default {
  components: {
        PopUpGanaste,
        PopUpPerdiste,
  },
  computed: {
    tiempoFormateado() {
      return String(this.tiempo).padStart(2, '0');
    }
  },
  data() {
    return {
      cartas: [],
      primeraCarta: null,
      segundaCarta: null,
      bloqueo: false,
      tiempo: 30,
      temporizador: null,
      ganaste: false,
      perdiste: false,
      mezclando: false,
      juegoSeleccionado: 'memoria',
      mostrarPopup: false,
      parejasEncontradas: 0, 
    };
  },
  created() {
    this.reiniciarJuego();
  },
  methods: {
    getImagenCarta(valor) {
      return `${import.meta.env.BASE_URL}images/${valor}.png`;
    },
    aceptarGanaste() {            
            this.ganaste = false
            this.$router.push('/')
        },
    home(){this.$router.push('/')},
    cerrarPopup() {
      this.mostrarPopup = false
    },
    reiniciarPerdiste() {
      this.perdiste = false
      this.reiniciarJuego();
    },
    salirPerdiste() {
      this.perdiste = false
      this.$router.push('/')
    },
    iniciarJuego() {
      this.mostrarPopup = false
      this.reiniciarJuego();
    },
    generarCartas() {
      const width = window.innerWidth;
      const height = window.innerHeight;

      const valoresBase = ['card1','card2','card3','card4','card5','card6'];
      const valores = [...valoresBase, ...valoresBase];

      return valores
        .map(valor => ({
          valor,
          volteada: false,
          encontrada: false,
          dx: (Math.random() * 0.4 - 0.2) * width,
          dy: (Math.random() * 0.4 - 0.2) * height,
          rot: (Math.random() * 20 - 10)
        }))
        .sort(() => 0.5 - Math.random());
    },
    voltearCarta(index) {
      if (this.ganaste || this.perdiste) return;
      const carta = this.cartas[index];
      if (this.bloqueo || carta.volteada || carta.encontrada) return;

      carta.volteada = true;

      if (!this.primeraCarta) {
        this.primeraCarta = { carta, index };
      } else if (!this.segundaCarta) {
        this.segundaCarta = { carta, index };
        this.bloqueo = true;

        setTimeout(() => {
          const { carta: primera } = this.primeraCarta;
          const { carta: segunda } = this.segundaCarta;

          if (primera.valor === segunda.valor) {
            primera.encontrada = true;
            segunda.encontrada = true;
            this.parejasEncontradas++;
          } else {
            primera.volteada = false;
            segunda.volteada = false;
          }

          this.primeraCarta = null;
          this.segundaCarta = null;
          this.bloqueo = false;

          if (this.cartas.every(c => c.encontrada)) {
            this.ganaste = true;
            clearInterval(this.temporizador);            
          }
        }, 700);
      }
    },
    iniciarTemporizador() {
      this.temporizador = setInterval(() => {
        if (this.ganaste) {
          clearInterval(this.temporizador);
          return;
        }

        if (this.tiempo > 0) {
          this.tiempo--;
        } else {
          clearInterval(this.temporizador);
          this.perdiste = true;
        }
      }, 1000);
    },
    reiniciarJuego() {
      this.cartas = this.generarCartas();
      this.primeraCarta = null;
      this.segundaCarta = null;
      this.bloqueo = true;
      this.tiempo = 30;
      this.ganaste = false;
      this.perdiste = false;
      this.parejasEncontradas = 0;
      clearInterval(this.temporizador);

      this.mezclando = true;
      setTimeout(() => {
        this.mezclando = false;
        this.bloqueo = false;
        this.iniciarTemporizador();
      }, 1500);
    }
  }
};
</script>
<style scoped>
.juego-memoria {
  width: 100vw;
  height: 100vh;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.fondo {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 0;
}

.contenido {
  position: relative;
  z-index: 1;
  flex: 1;
  padding: 4vh 2vw;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: flex-start;
  text-align: center;
  color: white;
  font-weight: bold;
}

.temporizador {
  color: #000;
  display: flex;
  justify-content: center; 
  align-items: center;
  gap: 10vh;
  padding: 1vh 4vh;
  font-size: 6vh;

  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-color: #FFF;
  border-radius: 2vh;
    
  align-self: center;
  min-width: 5ch; 
  font-family: 'EmigreTitulo', sans-serif !important;
   
}

.temporizador span {
  font-family: 'EmigreTitulo', sans-serif;
  display: inline-block;
  width: 5ch;
  text-align: center;
}

.tablero {
  margin-top: 3vh;
  display: grid;
  gap: 0.3vh;
  grid-template-columns: repeat(4, 1fr);
  width: min(90vw, 90vh);
  flex-grow: 2;
  perspective: 1000px;
  
}

.carta {  
  width: 100%;
  aspect-ratio: 3.5/5;
  padding: 0.3vh;
  filter: drop-shadow(0 0.1vh 0.3vh rgba(0,0,0,0.5));
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.6s ease;
  transform-style: preserve-3d;  
  border-radius: 1vh;
  background: transparent;
  transform: translateZ(0);
}

.card-inner.girada {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 1vh;
  overflow: hidden; /* evita que la imagen se salga */

  padding: 0; /* 👈 por si acaso */

  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;

  background: transparent;
  
}

.card-front img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 1vh;  
}

.card-back img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  z-index: 2;
}

.card-back {
  transform: rotateY(180deg);
  align-items: center;     
  justify-content: center; 
  display: flex;  
}

.mezclando {
    animation: shuffleCasinoPro 0.5s cubic-bezier(0.3, 1.3, 0.4, 1) forwards;
    will-change: transform;
    transform: translateZ(0);
    animation-timing-function: linear;
}

@keyframes shuffleCasinoPro {
  0% {
    transform: translate(0, 0) scale(1) rotate(0deg);
  }

  25% {
    transform: translate(var(--dx), var(--dy)) scale(0.8) rotate(var(--rot));
  }

  50% {
    transform: translate(calc(var(--dx) * -1), calc(var(--dy) * -1)) scale(0.8) rotate(calc(var(--rot) * -1));
  }

  75% {
    transform: translate(0, 0) scale(1.02) rotate(2deg);
  }

  /* Normal */
  100% {
    transform: translate(0, 0) scale(1) rotate(0deg);
  }
}

.area-juego {
  width: 90vw;
  /* igual que el tablero */
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 1vh;
}

/* Botones fijos abajo */
.acciones {
  display: flex;
  justify-content: flex-end;
  width: 85vw;
  margin: 1vh auto 0;
}

.boton-icono {
  background-color: white;
  border: none;
  border-radius: 50%;
  width: 7vh;
  height: 7vh;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 6px rgba(217, 87, 87, 0.25);
  cursor: pointer;
  transition: transform 0.2s ease;
}

.boton-icono:hover {
  transform: scale(1.1);
}

.boton-icono img {
  width: 5vh;
  height: 5vh;
  object-fit: contain;
}

.cslogan-img{
  max-width: 70%;
  margin: 0.5vh auto 2vh;
}

/* 📱 Adaptación a pantallas pequeñas */
@media (max-width: 768px) {
  .contenido h2 {
    font-size: 3vh;
  }

  .temporizador {
    font-size: 2.5vh;
  }

  .tablero {
    grid-template-columns: repeat(4, 1fr);
    gap: 1.5vh;
  }

  .acciones {
    gap: 4vw;
  }

  .boton-icono {
    width: 7vh;
    height: 7vh;
  }

  .boton-icono img {
    width: 3.5vh;
    height: 3.5vh;
  }
}
</style>