<template>
  <div>
    
    <!--{{this.$route.params.movie_pk}}-->
    
    <carousel-3d :width="300" :height="420" :disable3d="true" :space="365" :clickable="false" :controls-visible="true" >
    <slide v-for="(slide, i) in slides" :index="i" :key="slide">
    
    <div id="wrap">
    <img :src='`https://image.tmdb.org/t/p/original/${relatedMovieList[i].poster_path}`' alt="">
    <div class="title">
      <StarRating class="more justify-content-center"
            active-color="#FF8C0A"
            :show-rating="false"
            :rating="parseFloat(relatedMovieList[i].vote_average) / 2"
            :read-only="true"
            :increment="0.01"
            :star-size="40"
            />
      <!--<router-link class="more" :to="{name : 'MovieDetail', params : {movie_pk : relatedMovieList[i].id }}">Detail</router-link>-->
      <a class="more" :href='`http://localhost:8080/movies/${relatedMovieList[i].id}`' >상세 페이지</a>
    </div>
  </div>
    
    </slide>
  </carousel-3d>
  </div>
</template>

<script>
import { Carousel3d, Slide } from 'vue-carousel-3d'

import StarRating from "vue-star-rating";
export default {
  name: "MovieDetailRelated",
  data: function() {
    return {
        relatedMovieList: [],
        slides: 10
    }
  },
  methods: {
    getRelatedMovies: function() {
      const movie_data = JSON.parse(localStorage.getItem('movie_list'))[this.$route.params.movie_pk - 1]
      const wanted_genres = movie_data.genres
    
      
        for( let i = 0; i < 980; i ++ ){
          const sample_genres = JSON.parse(localStorage.getItem('movie_list'))[i].genres
          for(let k = 0; k < wanted_genres.length; k++){
            if (sample_genres.includes(wanted_genres[k])){
              if (i !== this.$route.params.movie_pk - 1){
                this.relatedMovieList.push(JSON.parse(localStorage.getItem('movie_list'))[i])
                break
              }
            }
              
          }
          if (this.relatedMovieList.length === 80){
              break
            }
        }

      this.relatedMovieList.sort(() => Math.random() - 0.5)


      
    },
      /*
        axios
        .get(
            `https://api.themoviedb.org/3/movie/${this.$route.params.movie_pk}/recommendations?api_key=5d2592924ae354925561438e12ee8888&language=en-US&page=1`
        )

        .then((res)=>{
            console.log(res.data)
            for (let i = 0; i < 10; i++) {
                this.relatedMovieList.push(res.data.results[i])
            }
            console.log(this.relatedMovieList)
        })
        .catch((err) => {
          console.log(err);
        });*/
        //let movie_genre_list = 1
    },
  
  components: {
    Carousel3d,
    Slide,
    StarRating,
  },
  created() {
    
    this.getRelatedMovies()
  }
}
</script>

<style>

#wrap_for_top {margin:0 auto; text-align:center; width:450px;}
.title {line-height:1; color:#E6FFE6; position:absolute; left:50%; transform:translateX(-50%); top:200px; height:120px; transition:0.5s all}
.title h3 {font-size:30px;  margin:0}
.title p {font-size:14px; margin-top:15px;}
.more {display:block; font-size:18x; color:#FF8C0A; background:#E6FFE6; line-height:60px; width:220px; margin-top:30px; opacity:0; transition:0.5s all}

#wrap_for_top:hover .more {opacity:0.7}
#wrap_for_top:hover .title {top:150px}

#wrap {margin:0 auto; text-align:center; width:300px;}

.title {line-height:1; color:#E6FFE6; position:absolute; left:50%; transform:translateX(-50%); top:200px; height:120px; transition:0.5s all}
.title h3 {font-size:30px;  margin:0}
.title p {font-size:14px; margin-top:15px;}
.more {display:block; font-size:18x; color:#FF8C0A; background:#E6FFE6; line-height:60px; width:220px; margin-top:30px; opacity:0; transition:0.5s all }

#wrap:hover .more {opacity:0.7}
#wrap:hover .title {top:150px}

</style>