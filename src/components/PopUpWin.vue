<template>
  <div v-if="visible" class="popup-overlay">
    <div class="popup-box">
      <div class="popup-fondo">
        <img
          src="/images/ganaste.png"
          alt="icono popup"
          class="popup-icono"
        />

        <h2 class="titulo-popup">
          ¡Felicidades!
        </h2>

        <div class="contenido-popup">
          <p>Lograste completar el reto</p>
          <p class="mensaje-final">en 60 segundos</p>
          <p>¡Disfruta de tu premio al instante!</p>
        </div>

        <button class="boton-iniciar" @click="iniciar">Aceptar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    visible: Boolean
  },
  emits: ['iniciar'],
  methods: {
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
  box-shadow: 0 4px 12px rgba(207, 60, 5, 0.7);
  text-align: center;
}

.popup-fondo {
  background-image: url('/images/fondomodal.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.popup-box h2 {
  font-size: 4.3vh;
  color: #EB6608;
  font-weight: 900; 
  line-height: 1.2;
  margin-bottom: 2vh;
}

.titulo-popup {
  font-size: 2.8vh; /* más pequeño */
  color: #EB6608;  
  margin-bottom: 2vh;
  font-family: 'EmigreTitulo', sans-serif;
}

.contenido-popup {
  font-size: 2vh;
  color: #31173b;
  text-align: center; /* ← centramos los párrafos */
  margin-bottom: 2vh;
  margin-top: 4vh;  
  font-family: 'EmigreTxt', sans-serif;
}

.contenido-popup ol {
  padding-left: 1.4em;
  margin: 1vh 0;
}

.contenido-popup p{
  margin: 0.4vh 0;
}

.mensaje-final {
  text-align: center;
  font-size: 2.2vh; /* más pequeño */
  margin-top: 2vh;
  color: #EB6608
}

.boton-iniciar {
  background-color: #EB6608;
  color: #fff;
  border: none;
  padding: 1.4vh 2.8vh;
  border-radius: 1vh;
  font-size: 2vh; /* más pequeño */
  cursor: pointer;
  width: 100%;
  transition: background 0.2s ease;
  margin-top: 2vh;
  font-family: 'EmigreTitulo', sans-serif;
}

.boton-iniciar:hover {
  background-color: #eb6708cf;
}

.popup-icono {
  object-fit: contain;
  margin: 0 auto 0.5svh;
  display: block;
  animation: animacionHamburguesa 1.2s infinite ease-in-out;
}

@keyframes animacionHamburguesa {
  0% {
    transform: scale(1) rotate(0deg) translate(0, 0);
    filter: brightness(1);
  }
  25% {
    transform: scale(1.03) rotate(1deg) translate(-1px, 1px);
    filter: brightness(1.1);
  }
  50% {
    transform: scale(1.05) rotate(-1deg) translate(1px, -1px);
    filter: brightness(1.2);
  }
  75% {
    transform: scale(1.03) rotate(1deg) translate(-1px, 0px);
    filter: brightness(1.1);
  }
  100% {
    transform: scale(1) rotate(0deg) translate(0, 0);
    filter: brightness(1);
  }
}

</style>