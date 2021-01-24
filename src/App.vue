


<template>
  <div class="corpo">
    <h2 class="centralizado" v-text="titulo" />

    <input
      type="search"
      class="filtro"
      v-on:input="filtro = $event.target.value"
      placeholder="filtre por titulo"
    />
    {{ filtro }}

    <ul class="lista-fotos">
      <li
        class="lista-fotos-item"
        :key="foto.titulo"
        v-for="foto of fotosComFiltro"
      >
        <meu-painel :titulo="foto.titulo" :src="foto.url">
          <img :src="foto.url" :alt="foto.titulo" />
        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
import api from "./services/api";

import Painel from "./components/shared/Painel/Painel";

export default {
  components: {
    "meu-painel": Painel,
  },

  data() {
    return {
      titulo: "Alurapic",

      fotos: [],

      filtro: "",
    };
  },

  computed: {
    fotosComFiltro() {
      if (this.filtro) {
        let exp = new RegExp(this.filtro.trim(), "i");

        return this.fotos.filter((foto) => {
          return exp.test(foto.titulo);
        });
      }

      return this.fotos;
    },
  },

  created() {
    try {
      api.get("/v1/fotos").then((response) => {
        console.log(response.data);

        this.fotos = response.data;
      });
    } catch (err) {
      console.log(err.message);
    }

    // this.$http
    //   .get("http://localhost:3000/v1/fotos")
    //   .then((response) => (this.fotos = response.data));
  },
};
</script>

<style>
.corpo {
  margin: 0 auto;
  outline: 0;
  height: 100vh;
  font-family: Helvetica, sans-serif;
  width: 96%;
}

.centralizado {
  text-align: center;
}

.filtro {
  display: block;
  width: 100%;
}

.lista-fotos {
  display: grid;

  grid-template-columns: 1fr 1fr 1fr;

  grid-gap: 30px;
}

.lista-fotos-item {
  height: 230px;
  list-style-type: none;
}

.lista-fotos-item img {
  width: 100%;
}
</style>
