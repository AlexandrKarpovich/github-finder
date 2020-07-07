<template lang="pug">
    .wrapper-content.wrapper-content--fixed
      section
        .container
          //errors
          .error(style='margin-bottom: 20px;')
            p {{ error }}
          //seacrch
          search(:value='search', placeholder='Type user name...', @search='search = $event')
          // buttons
          button.btn.btnPrimary(v-if='!repos', @click='getRepos') Search!
          button.btn.btnPrimary(v-else='', @click='getRepos') Search gain!
          //wrapper
          .repos__wrapper(v-if='repos')
            //item
            .repos-item(v-for='repo in repos', :key='repo.id')
              .repos-info
                a.link(target='_blank', :href='repo.html_url') {{ repo.name }}
                span {{ repo.stargazers_count }} ⭐
</template>

<script>
import search from '@/components/Search.vue'
import axios from 'axios'

export default {
  components: { search },
  data () {
    return {
      search: '',
      error: null,
      repos: null
    }
  },
  methods: {
    getRepos () {
      axios
        .get(`https://api.github.com/users/${this.search}/repos`)
          .then(res => {
            // console.log(res)
            this.repos = res.data
            this.repos = res.data
          })
          .catch(err => {
            console.log(err)
            this.repos = null
            this.error = 'Can`t find this user'
          })
    }
  }
}
</script>

<style lang="stylus" scoped>
.container
  display: flex
  align-items: center
  flex-direction: column
button
  margin-top: 40px
.repos__wrapper
  width: 600px
  margin: 30px 0
.repos-info
  display: flex
  align-items: center
  justify-content: space-between
  margin-bottom: 10px
  padding: 10px 0
  border-bottom: 1px solid #dbdbdd
</style>