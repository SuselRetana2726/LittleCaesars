<template>
  <div v-if="visible" class="popup-overlay">
    <div class="popup-box">
      <div class="popup-fondo">
        <img
          src="/images/perdiste.png"
          alt="icono popup"
          class="popup-icono"
        />

        <h2 class="titulo-popup">
          ¡UPS, CASI LO LOGRAS!
        </h2>

        <div class="contenido-popup">
          <p>Te faltó muy poquito para disfrutar de la pizza...</p>
        </div>

        <div class="contenido-popup" style="margin-top: 0vh !important;">
          <p class="resultado">
            <banner class="naranja">Has encontrado </banner><strong>{{ parejas }}</strong><banner class="naranja"> parejas</banner>
          </p>
        </div>

        <div class="botones-popup">
          <button class="boton-iniciar" @click="iniciar">Reiniciar</button>
          <button class="boton-salir" @click="cerrar">Salir</button>
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  props: {
    visible: Boolean,
    parejas: {
      type: Number,
      default: 0
    }
  },
  emits: ['cerrar', 'iniciar'],
  methods: {
    cerrar() {
      this.$emit('cerrar');
    },
    iniciar() {
      this.$emit('iniciar');
    }
  }
};
</script>

<style scoped>
.popup-overlay {  
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(207, 60, 5, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}

.popup-box {    
  position: relative;
  background: #ffffff;
  border-radius: 2vh;
  padding: 3vh 2.5vh;
  width: 55%;
  max-width: 60vw; 
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25);
  text-align: center;
}

.popup-fondo {
  background-image: url('/images/fondomodal.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.popup-box h2 {
  font-size: 3vh;
  color: #EB6608;
  font-weight: 900; 
  line-height: 1.2;
  margin-bottom: 2vh;
  text-transform: uppercase;
}

.titulo-popup {
  font-size: 2.8vh; /* más pequeño */
  color: #EB6608;
  margin-bottom: 2vh;
  text-transform: uppercase;
  font-family: 'EmigreTitulo', sans-serif !important;
}

.contenido-popup {
  font-size: 1.8vh;
  color: #1A1413;
  text-align: center; /* ← centramos los párrafos */
  margin-bottom: 1vh;
  margin-top: 4vh;  
  font-family: 'EmigreTxt', sans-serif !important
}

.contenido-popup p{
  margin: 0.4vh 0;
}
.contenido-popup ol {
  padding-left: 1.4em;
  margin: 1vh 0;
}

.botones-popup {
  display: flex;
  gap: 1vh;
  justify-content: center;
  margin-top: 3vh;
  font-family: 'EmigreTitulo', sans-serif !important
}

.botones-popup button {
  width: 45%;
  font-family: 'EmigreTitulo', sans-serif !important
}

.boton-iniciar,
.boton-salir {
  flex: 1;  
  border: none;
  padding: 1.4vh 2.8vh;
  border-radius: 1vh;
  font-size: 2vh;
  cursor: pointer;
  transition: background 0.2s ease;
}

.boton-iniciar {
  background-color: #EB6608;
  color: #fff;
}

.naranja{
  color: #EB6608;
}
.boton-salir {
  background-color: #1A1413;
  color: #fff;
}

.popup-icono {
  object-fit: contain;
  margin: 0 auto 0.5svh;
  display: block;
  animation: sacudidaError 0.4s ease-in-out 1, flameLoop 1.6s ease-in-out infinite;
}

@keyframes caerRebotar {
  0% {
    transform: translateY(-40px);
    opacity: 0;
  }
  60% {
    transform: translateY(10px);
  }
  80% {
    transform: translateY(-5px);
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

</style>