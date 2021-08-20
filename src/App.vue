<template>
  <div id="app">
    <h1>Random GIF Cat</h1>
    <form @submit.prevent="handleOnSubmit">
      <div style="background-color: coral">
        <div class="bloque">
          <label>Titulo:</label>
          <input type="text" v-model="form.titulo" required placeholder="Escriba un título" />
        </div>
        <div class="bloque">
          <label>Filtro:</label>
          <select v-model="form.filtro" required>
            <option disabled selected value="">Elige un filtro</option>
            <option value="blur">Blur</option>
            <option value="mono">Mono</option>
            <option value="sepia">Sepia</option>
            <option value="negative">Negative</option>
            <option value="paint">Paint</option>
            <option value="pixel">Pixel</option>
          </select>
        </div>
        <div class="bloque">
          <label>Color:</label>
          <select v-model="form.color" required>
            <option disabled selected value="">Elige un color</option>
            <option value="red">Rojo</option>
            <option value="blue">Azul</option>
            <option value="green">Verde</option>
            <option value="orange">Naranja</option>
          </select>
          <div class="circulo" :style="{ backgroundColor: this.form.color }"></div>
        </div>
        <div class="bloque">
          <label>Tamaño:</label>
          <input type="number" v-model.number="form.tamaño" required placeholder="Ingresa un tamaño" />
        </div>
      </div>
      <button type="submit" style="margin-top: 10px">{{ cargando ? 'Cargando gatito' : 'Obtener mi gatito' }}</button>
    </form>
    <img :src="urlGatito" class="gatito" />
  </div>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    form: { titulo: '', filtro: '', color: '', tamaño: '' },
    urlGatito: '',
    cargando: false,
  }),
  methods: {
    async handleOnSubmit() {
      this.cargando = true
      try {
        const response = await fetch(`https://cataas.com/cat/gif/says/${this.form.titulo}?${this.formParams}`)
        const Blob = await response.blob()
        const Imagen = URL.createObjectURL(Blob)
        this.urlGatito = Imagen
        /* const Data = response.json()
        console.log(Data) */
      } catch (e) {
        console.error(e)
      } finally {
        this.cargando = false
      }
    },
  },
  computed: {
    formParams() {
      return `filter=${this.form.filtro}&color=${this.form.color}&size=${this.form.tamaño}`
    },
  },
}
</script>

<style lang="scss">
html {
  background-color: lightblue;
  margin: 0;
  padding: 0;
  border: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 0;
}
.bloque {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 7px;
}
.circulo {
  background-color: lightcoral;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-left: 10px;
}
input,
select {
  margin-left: 10px;
}
</style>
