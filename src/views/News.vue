<template>
  <v-app class="busca" id="inspire">
    <v-app-bar app shrink-on-scroll>
      <!-- <v-app-bar-nav-icon></v-app-bar-nav-icon> -->

      <v-toolbar-title>Feed de Notícias</v-toolbar-title>

      <v-spacer></v-spacer>

      <!-- <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn> -->
    </v-app-bar>
    <div class="news">
      <p>Pesquisa de Notícias</p>
      <form class="form" v-on:submit.prevent="searchNews">
        <input
          type="text"
          class="assunto-input"
          v-model="assunto"
          placeholder="Assunto"
          maxlength="8"
        />
        <input type="submit" value="Pesquisar" class="assunto-button" />
      </form>
      <p>{{ counter }}</p>
      <CardNews :data="data" :texto="texto"></CardNews>
    </div>
  </v-app>
</template>

<script>
import axios from "axios"; // biblioteca usada para fazer requisições HTTP, similar ao Fetch
import CardNews from "../components/CardNews";

export default {
  name: "News",
  data() {
    return {
      texto: "",
      assunto: "",
      data: {
        count: 0,
        items: [
          {
            titulo: "",
            introducao: "",
            data_publicacao: "",
            link: "",
          },
        ],
      },
      status: Number,
      isError: false,
    };
  },
  methods: {
    searchNews: function() {
      if (this.assunto.length > 0) {
        axios
          .get(
            `http://servicodados.ibge.gov.br/api/v3/noticias/?busca=${this.assunto}/json`
          )
          .then((response) => {
            this.status = response.status;
            this.data = response.data;
            this.texto = "Leia Mais 👉";
            console.log(response);
          })
          .catch(
            // caso a Promise seja rejeitada cairá no catch
            (error) => {
              console.log(error);
              this.isError = true;
            }
          );
      }
    },
  },
  computed: {
    counter: function() {
      return `${this.data.count} notícias encontradas`;
    },
  },
  components: {
    CardNews,
  },
};
</script>

<style scoped>
.busca {
  padding: 30px;
}

.assunto-input {
  border: 1px solid black;
  border-radius: 5px;
  padding: 5px;
  width: 250px;
  margin: 10px;
}

.assunto-button {
  border: 1px solid black;
  border-radius: 5px;
  width: 100px;
  background-color: teal;
  color: white;
  margin: 10px;
}

.city-button:hover {
  background-color: rgb(155, 155, 155);
}
</style>
