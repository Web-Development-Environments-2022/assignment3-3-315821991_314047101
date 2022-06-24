<template>

  <div>
    <h1 class="title">Favorite Games</h1>
    <br/><br/>

    <FavoriteRecipes v-bind:recipe="recipe"></FavoriteRecipes>

  </div>

</template>

<script>
import FavoriteRecipes from "../components/FavoriteRecipes";
export default {
  components: { 
    FavoriteRecipes
  }, 
  data() {
    return {
      recipe: []
    };
  },
  methods: {
    getFavoriteRecipes: async function() {
      try {
        const favoriteRecipes = await this.axios.get(
        'http://localhost:3000/users/favorites'
        );
        for (const i in favoriteRecipes.data) {
          if (favoriteRecipes.data[i] != undefined) {
            this.recipe.push(favoriteRecipes.data[i]);
          }
        }
      } catch (err) {
        console.log(err.response);
      }
    }
  },
  mounted(){
    this.getFavoriteRecipes();
  } 
};
</script>


<style scoped>
/* Style the buttons */
.btn {
  border: none;
  outline: none;
  padding: 10px 16px;
  background-color: #f1f1f1;
  cursor: pointer;
}

/* Style the active class (and buttons on mouse-over) */
.active, .btn:hover {
  background-color: #666;
  color: white;
}
#search-input {
  margin-left: 20px; 
  width: 500px; 
}
</style>