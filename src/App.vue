<template>
  <div id="app">
    <div class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">R&M</span>
          <span class="subtitle">Personajes</span>
        </h1>

        <div class="field has-addons is-pulled-right">
          <div class="control">
            <input
              type="text"
              v-model="search"
              class="input is-rounded"
              v-on:keyup.enter="searhData"
            />
          </div>
          <div class="control">
            <button class="button is-success is-rounded" v-on:click="searhData">
              Buscar
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="columns is-desktop is-tablet is-miltiline is-centered">
        <charater
          @showModal="showModal"
          v-for="character of characters"
          :key="character.id"
          :character="character"
        />
      </div>

      <nav class="pagination" role="navegation" aria-label="pagination">
        <a class="pagination-previous" v-on:click="changePage(page - 1);"
          >Anterior</a
        >
        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current">{{ page }}</a>
          </li>
        </ul>
        <a class="pagination-next" v-on:click="changePage(page + 1);"
          >Siguiente</a
        >
      </nav>
    </div>

    <div class="modal" :class="{ 'is-active': modal }" v-if="modal">
      <div class="modal-background" @click="modal = false;"></div>

      <div class="modal-card">
        <div class="modal-card-head">
          <p class="modal-card-title">Acerca de: {{ personaje.name }}</p>
        </div>
        <div class="modal-card-body">
          <p>Genero: {{ personaje.gender }}</p>
          <p>estado: {{ personaje.status }}</p>
          <p>raza: {{ personaje.species }}</p>
          <p>tipo: {{ personaje.type }}</p>
        </div>
        <footer class="modal-card-food">
          <button class="button" @click="modal = false;">cerrar</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
//librerias
import axios from "axios";
import Charater from "./components/Charater.vue";
export default {
  name: "App",
  components: {
    Charater
  },
  data: function() {
    return {
      characters: [],
      page: 1,
      pages: 1,
      search: "",
      modal: false,
      personaje: []
    };
  },
  created() {
    this.fech();
  },

  methods: {
    fech() {
      let url = "https://rickandmortyapi.com/api/character";

      let result = axios
        .get(url, {
          params: {
            page: this.page,
            name: this.search
          }
        })
        .then(reponse => {
          this.characters = reponse.data.results;
          this.pages = reponse.data.info.pages;
          console.log(reponse.data.info);
        })
        .catch(error => {
          console.log(error);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fech();
    },
    searhData() {
      this.page = 1;
      this.fech();
      console.log(this.search);
    },
    showModal(id) {
      this.fetchOne(id);
    },
    async fetchOne(id) {
      let result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}/`
      );

      this.personaje = result.data;
      this.modal = true;
      console.log(this.personaje);
    }
  }
};
</script>
