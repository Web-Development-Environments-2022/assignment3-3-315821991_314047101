<!-- <template>

  <div>
    <h1 class="title">Favorite Games</h1>
    <br/><br/>

    <FavoriteRecipes v-bind:recipe="recipe"></FavoriteRecipes>

  </div>

</template> -->


<template>
  <b-container>
 <h1 class="title">Favorite Recipes</h1>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "../components/RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/favorites", { withCredentials: true }
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );

        // console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}

h1{
    text-align:center; 
    color: rgb(182, 99, 22);
    font-family:'Gill Sans', 'Gill Sans MT';
    background-color: rgb(245, 234, 212);
    padding: 20px;
}
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


<!-- 
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
</script> -->


<!-- 
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
</style> -->