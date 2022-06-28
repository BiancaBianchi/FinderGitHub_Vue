<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="search card card-body">
        <h1>Pesquisar Usuários do GitHub</h1>
        <p class="lead">
          Digite um nome para encontrar usuários e repositórios
        </p>
        <input
          @keyup="getUser"
          class="form-control"
          placeholder="Digite o nome de um usuário..."
          required
        />
      </div>

      <div
        class="row mt-3"
        v-if="user.length !== 0"
      >
        <div class="col-md-4">
          <Profile :user="user" />
        </div>
        <div class="col-md-8">
          <Repos
            v-for="repo in repos"
            :repo="repo"
            :key="repo"
          />
        </div>
      </div>
     
      
        <div class="search card card-body">
          <div class="input">
            <h2>Clima na sua cidade</h2>
          <input type="text" placeholder="Digite a cidade" class="input_text" style="width: 1010px; border: 1px solid #ced4da;" >
          <input type="submit" value="Submit" class="submit" style="background-color: #198754; color: white; border-radius: 5px;">
        
      
        </div>
      </div>
    
  
    </div>
    <div class="container">
      <div class="card" style="padding: 20px;">
        <h2 class="name" id="name"></h2>
        <p class="temp"></p>
        <p class="clouds"></p>
        <p class="desc"></p>
      </div>
    </div>

  </div>

</template>

<script>
import axios from "axios";
import Navbar from "./components/Navbar.vue";
import Profile from "./components/Profile.vue";
import Repos from "./components/Repos.vue";
export default {
  name: "app",
  data() {
    return {
      github: {
        url: "https://api.github.com/users",
        client_id: "efe6d142e68b2dfa82f6",
        client_secret: "bd7e2fcd4bddfe163dae4a5df17c0830556bb3f2",
        count: 7,
        sort: "created: asc"
      },
      user: [],
      repos: []
    };
  },
  components: {
    Navbar,
    Profile,
    Repos
  },
  methods: {
    getUser(e) {
      const user = e.target.value;
      const { url, client_id, client_secret, count, sort } = this.github;
      axios
        .get(
          `${url}/${user}?client_id=${client_id}&client_secret=${client_secret}`
        )
        .then(({ data }) => (this.user = data));
      axios
        .get(
          `${url}/${user}/repos?per_page=${count}&sort=${sort}&client_id=${client_id}&client_secret=${client_secret}`
        )
        .then(({ data }) => (this.repos = data));
    }
  }
};
</script>