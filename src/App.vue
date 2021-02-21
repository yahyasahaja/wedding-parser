<template>
  <div id="app">
    <textarea
      v-model="urlsInput"
      class="input-text"
      cols=10
    />
    <button @click="clear">
      hapus semua
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
          const [name, time] = str.split(',').map(s => s.trim());
          if (!name || !time) return null
          return {
            name,
            time
          }
        })
        .filter(data => data)
    },
    links() {
      return this.linkData
        .map(({ name, time }) =>
          new URL(`https://adhisazis.netlify.app/?jam=${time}&nama=${name}`).toString()
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

.input-text {
  max-width: 350px;
  padding: 10px;
  width: 100%;
  margin: 10px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 14pt;
  border: 1px solid #7d7d7d;
  border-radius: 10px;
  min-height: 300px;
}
</style>
