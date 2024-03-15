<template>
  <div>
    <header>
      <h1>The <strong>Movie</strong> Database</h1>
      <form action="" class="search-box" v-on:submit.prevent="handleSearch">
        <input 
          type="search"
          placeholder="Search for a movie..."
          v-model="search_query" 
          required 
        >
      </form>
    </header>
    <main>
      <div class="cards" v-if="movieList.length > 0 ">      
        <Card v-for="movie in movieList" v-bind:aa="movie"/>
        <!-- aa라는 이름으로 movie가 전달됨 -->
      </div>
      <div class="no-result" v-else>
        <h3>No search results for '{{ search_query }}'</h3>
      </div>
    </main>
  </div>
</template>

<script setup>
import {ref} from 'vue';
import Card from './components/Card.vue';

const movieList = ref([]);
const search_query = ref('');

const handleSearch = async () => {
  movieList.value = await fetch(`https://api.themoviedb.org/3/search/movie?query=${search_query.value}&include_adult=false&language=en-US&page=1&api_key=8aa0866155998478fa5ad874b3d218a1`)
    .then(response => response.json())
    .then(response => response.results)
    console.log('받아온 데이타', movieList.value)
}

// const popular = async () => {
//   movieList.value = await fetch('https://api.themoviedb.org/3/movie/popular?language=en-US&page=1&api_key=8aa0866155998478fa5ad874b3d218a1')
//     .then(response => response.json())
//     .then(response => response.results)
//     console.log('받아온 데이타', movieList.value)
// }
// popular();


</script>

<style lang="scss" scoped>
  $color:#313131;

  * { 
    margin: 0;
    padding:0;
    box-sizing: border-box;
    font-family: sans-serif;
  }

  header {
    padding:50px 0;

    h1 {
      color:#888;
      font-size: 42px;
      font-weight: 400;
      text-align: center;
      margin-bottom: 30px;
      text-transform: uppercase;

      strong{
        color:$color
      }
    }

    .search-box{
      display: flex;
      justify-content: center;

      input {
        appearance: none;
        border:none;
        outline:none;
        background:#f3f3f3;

        padding:15px;
        width:100%;
        max-width:600px;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.2);
        color:$color;
        font-size: 20px;
        transition:0.3s ease-in;

        &::placeholder {
          color: #aaa;
        }
        &:focus {
          background: $color;
          color: #fff;
        }
      }
    }
  }

  main {
    margin: auto;

    .cards {
      display: flex;
      flex-wrap: wrap;
      max-width:1400px;
      margin:auto;
    }
    .no-result{
      text-align: center;
      padding: 50px 0;
    }
  }
</style>

