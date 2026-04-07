<template>
  <router-view />

  <!-- Mantiene imágenes en memoria -->
  <div style="display:none">
    <img src="/images/logo.png" />
    <img src="/images/titulohome.png" />
    <img src="/images/insignia-home.png" />
    <img src="/images/fondo.jpg" />
    <img src="/images/mfondo.jpeg" />
    <img src="/images/card.png" />
    <img src="/images/card1.png" />
    <img src="/images/card2.png" />
    <img src="/images/card3.png" />
    <img src="/images/card4.png" />
    <img src="/images/card5.png" />
    <img src="/images/card6.png" />
  </div>
</template>

<script>
export default {
  name: 'App',

  mounted() {
    // 🔥 Precarga real (clave)
    const imagenes = [
      '/images/logo.png',
      '/images/titulohome.png',
      '/images/insignia-home.png',
      '/images/fondo.jpg',
      '/images/mfondo.jpeg',
      '/images/card.png',
      '/images/card1.png',
      '/images/card2.png',
      '/images/card3.png',
      '/images/card4.png',
      '/images/card5.png',
      '/images/card6.png'
    ]

    imagenes.forEach(src => {
      const img = new Image()
      img.src = import.meta.env.BASE_URL + src
    })
  }
}
</script>