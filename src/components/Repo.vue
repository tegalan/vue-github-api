<template>

    <div id="repo">
        <div class="col-md-8 col-md-offset-2">
            <p v-if="isLoading" class="alert alert-success">Loading detail repo...</p>
            <p v-if="isError" class="alert alert-danger">Galat memuat data repo</p>
            <div class="panel panel-default">
                <div class="panel-heading">{{repo.full_name}} by <a :href="'https://github.com/'+repo.owner.login">@{{repo.owner.login}}</a><span class="label label-success pull-right">{{repo.language}}</span></div>
                <div class="panel-body">
                    {{repo.description}}

                </div>
                <div class="panel-footer">
                    <a :href="repo.html_url" class="btn btn-success btn-sm" target="_blank">Check Repo on GitHub</a>
                    <a :href="repo.owner.html_url" class="btn btn-info btn-sm" target="_blank">Visit @{{repo.owner.login}} profile on GitHub</a>
                </div>
            </div>
        </div>
    </div>
    
</template>

<script>
import axios from 'axios'

export default {
  name: 'repo',
  data () {
    return {
      repo: {},
      isLoading: false,
      isError: false
    }
  },
  methods: {
    loadData: function () {
      let username = this.$route.params.username
      let repo = this.$route.params.repo

      this.isLoading = true

      axios.get(`https://api.github.com/repos/${username}/${repo}`)
        .then((response) => {
          console.log(response)
          this.repo = response.data
          this.isLoading = false
        }, (err) => {
          console.log(err)
          this.isError = true
          this.isLoading = false
        })
    }
  },
  mounted: function () {
    this.loadData()
  }
}
</script>
