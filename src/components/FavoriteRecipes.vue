<template>
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
</script>

<style>
.favorite{
  width: 400px;
  font-size: 35px;
  font-weight: bold;
  color: rgb(255, 255, 255);
}
</style>
