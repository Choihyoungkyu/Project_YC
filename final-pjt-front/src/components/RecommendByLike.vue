<template>
  <div class="row" style="height:100%;">
    <div class="row flex-nowrap" id="scollbar" style="height:100%">
      <span v-for="movie in Filterd_Movies" :key=movie[1].id style="width:190px; height:100%"> 
        <router-link :to="{name : 'MovieDetail', params : {movie_pk : movie[1].id }}">
          <img :src='`https://image.tmdb.org/t/p/original/${movie[1].poster_path}`' alt="" style="height:230px; width:180px;" class="rounded-2">
        </router-link>
      </span>
    </div>
  </div>
</template> 

<script>
import _ from 'lodash'
export default {
  name: 'RecommendByLike',
  props: {
    like_movies: Array,
    LawData: Object,  
  },
  data() { 
    return { 
      movies : JSON.parse(localStorage.getItem('movie_list')),
      filterd_movies : [],
      genre_id: [],
      genre_data: [],
    }
  },
  computed: {
    Genre_id_arr() {
      return this.genre_id
    },
    Genre_data() {
      return this.genre_data
    },
    Datas() {
      const datas = []
      for (const data in this.LawData) {
        if (datas.length < 3 && this.LawData[data] !== 0) {
          const obj = {}
          obj[data] = this.LawData[data]
          datas.push(obj)
        } else {
          break
        }
      }
      return datas
    },
    Filterd_Movies() {
      this.getFilteredMovies()
      return this.filterd_movies
    }
  },
  methods: {
    getGenres_id() {
      const genre_data = JSON.parse(localStorage.getItem('genres'))
      for (const data of this.Datas) {
        for (const gen in genre_data) {
          if (genre_data[gen]['name'] === Object.keys(data)[0]) {
            const tmp = {}
            tmp[genre_data[gen]['id']] = data[genre_data[gen]['name']]
            this.genre_data.push(tmp)
            // console.log(genre_data[gen]['id'])
            this.genre_id.push(genre_data[gen]['id'])
          }
        }
      }
    },
    getFilteredMovies() {
      this.getGenres_id()
      // const genre_id_arr = this.Genre_id_arr
      const genre_data = this.Genre_data
      for (const movie of this.movies) {
        let tmp = 0
        for (const gen of movie.genres) {
          for (const gen_data of genre_data) {
            if (Number(Object.keys(gen_data)[0]) === gen) {
              // console.log(this.like_movies.includes(movie))
              let flag = false
              for (const like_movie in this.like_movies) {
                // console.log(this.like_movies[like_movie].original_id, movie.original_id)
                if (this.like_movies[like_movie].original_id === movie.original_id) {
                  flag = true
                  break
                } 
              }
              if (!flag) {
                tmp += Object.values(gen_data)[0] * (movie.popularity/90) 
              }
            }
          }
        }
        if (tmp !== 0) {
          const arr = [tmp, movie]
          this.filterd_movies.push(arr)
        }
      }
      this.filterd_movies.sort((a, b) => b[0] - a[0])
      this.filterd_movies = this.filterd_movies.slice(0, 40)
      this.filterd_movies = _.sampleSize(this.filterd_movies, 10)
      this.getList()
    },
    getList() {
      this.filtered_movies = this.filterd_movies
    }
  },
}
</script>

<style>

</style>