<template>
  <div class="home">
    <div class="col-md-8 col-md-offset-2">

      <p v-if="repos.length <= 0 && isLoading == false" class="alert alert-warning">Tidak ada Data</p>
      <p v-if="isLoading == true" class="alert alert-success">Memuat data repositori...</p>
      <p v-if="isError == true" class="alert alert-danger">Error memuat data</p>

      <div v-for="repo of repos" class="panel panel-default">
          <div class="panel-heading">{{repo.full_name}} <span class="label label-success pull-right">{{repo.language}}</span></div>
          <div class="panel-body">
              {{repo.description}}
          </div>
          <div class="panel-footer"><router-link class="btn btn-info btn-sm" :to="{name: 'Repo', params: {username: repo.owner.login, repo: repo.name} }">Details</router-link></div>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'home',
  data () {
    return {
      repos: [],
      isLoading: false,
      isError: false
    }
  },
  methods: {
    loadRepos: function () {
      this.isLoading = true

      axios.get('https://api.github.com/users/showcheap/repos')
        .then((response) => {
          console.log(response)
          this.repos = response.data
          this.isLoading = false
        }, (err) => {
          console.log(err)
          this.isError = true
          this.isLoading = false
        })
    }
  },
  mounted: function () {
    this.loadRepos()
  }
}
</script>
