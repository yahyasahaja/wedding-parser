<template>
  <div id="app">
    <label>
      Format: nama (dipisah per-baris)
    </label>
    <textarea
      v-model="urlsInput"
      class="input-text"
      placeholder="Bapak Solihin"
      cols=10
    />
    <button class="button" @click="clear">
      Hapus Semua
    </button>
    <h3>
      Hasil (klik untuk copy):
    </h3>

    <div>
      <a
        style="margin-bottom: 10px; display: block"
        v-for="(link, i) in links"
        :key="i"
        href="#"
        @click="handleCopy($event, link, i)"
      >
        {{ link }}
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  computed: {
    linkData() {
      return this.urlsInput
        .split('\n')
        .map(str => {
          const [
            name,
            // time
          ] = str.split(',').map(s => s.trim());
          if (
            !name
            // || !time
          ) return null
          return {
            name,
            // time
          }
        })
        .filter(data => data)
    },
    links() {
      return this.linkData
        .map(({
          name,
          // time
        }) =>
          // new URL(`https://aldhynida.netlify.app/?jam=${time}&nama=${name}`).toString()
          new URL(`https://aldhynida.netlify.app/?nama=${name}`).toString()
        );
    }
  },
  data() {
    return {
      urlsInput: '',
    }
  },
  methods: {
    clear() {
      this.urlsInput = ''
    },
    handleCopy(e, link, i) {
      e.preventDefault();
      this.$clipboard(link);
      alert(`Undangan untuk ${this.linkData[i].name} telah berhasil dicopy`)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 12pt;
}

* {
  box-sizing: border-box;
}

.input-text {
  max-width: 350px;
  padding: 10px;
  width: 100%;
  margin: 10px auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 14pt;
  border: 1px solid #7d7d7d;
  border-radius: 10px;
  min-height: 300px;
  display: block;
}

.button {
  border: none;
  background: #f44336;
  color: white;
  padding: 10px 20px;
  border-radius: 30px;
}
</style>
