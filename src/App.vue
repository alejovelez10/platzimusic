<template lang="pug">
  #app
    img(src='https://alejovelez10.github.io/platzimusic/dist/logo.png')
    h1 PlatziMusic  
    select(v-model="selectedCountry")
      option(v-for="country in countries"
      v-bind:value="country.value") {{ country.name }} 
    spinner(v-show="louding")
    ul
      artist(v-for="artist in artists"
      v-bind:artist="artist" v-bind:key="artist.mbid")
      

</template>

<script>
import Artist from './components/Artist.vue'
import getArtists from './api'
import Spinner from "./components/Spinner.vue"
export default {
  name: 'app',
  data () {
    return {
      artists:[],
      countries:[
        {name: "Argentina", value: "argentina"},
        {name: "Colombia", value: "colombia"},
        {name: "España", value: "spain"}
      ],
      selectedCountry: "argentina",
      louding: true
      }
    },
    components:{
      Artist: Artist,
      Spinner: Spinner

    },
    methods:{
      refreshArtist(){
            const self = this
            this.louding = true
            getArtists(this.selectedCountry)
            .then(function(artists){
                self.louding = false
                self.artists = artists

            })

      }


    },
    mounted(){
        this.refreshArtist()
    },
    watch: {

        selectedCountry(){
            this.refreshArtist()

        }
    }
  }

</script>

<style lang="stylus">
#app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin-top 60px

h1, h2
    font-weight normal

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px

a
    color #42b983
</style>
