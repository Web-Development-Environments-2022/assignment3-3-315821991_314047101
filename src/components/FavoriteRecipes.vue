<!-- <template>
<div>
  <div class="favorite">
    Your Favorite Recipes:
    </div>
    <RecipePreview
    :recpiePrevieTest="recipe">
    </RecipePreview>

  </div>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "FavoriteRecipes",
  components: {
    RecipePreview
  }, 
  props: {
    recipe: {
      type: Array,
      required: true
      }
  },
};
</script> -->

<template>
  <div>
<RecipePreview
    v-for="r in recipes"
      :id="r.id"
      :title="r.title"
      :readyInMinutes="r.readyInMinutes"
      :image="r.image"
      :aggregateLikes="r.aggregateLikes"
      :key="r.id"
  >
  </RecipePreview>
  </div>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  //task 1
  name: "FavoriteRecipes",
  //task1.5
  components: {
    RecipePreview,
  },
  data() {
    return {
      recipes: [
        {
          id: 641726,
          title: "Dulce De Leche Brownies",
          readyInMinutes: 45,
          image: "https://spoonacular.com/recipeImages/641726-556x370.jpg",
          aggregateLikes: 29,
        },
        {
          id: 651389,
          title: "Medenjaci - Croatian Honey Spice Cookies",
          readyInMinutes: 45,
          image: "https://spoonacular.com/recipeImages/651389-556x370.jpg",
          aggregateLikes: 6,
        },
      ],
    };
  },
}
</script>



<style>
.favorite{
  width: 400px;
  font-size: 35px;
  font-weight: bold;
  color: rgb(255, 255, 255);
}
</style>
