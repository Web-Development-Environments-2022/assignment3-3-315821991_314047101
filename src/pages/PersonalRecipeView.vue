<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.aggregateLikes }} likes</div>
              <div>Number of servings: {{ recipe.servings }}</div>
            </div>
            Ingredients:
            <ul>
              <!-- <li
                v-for="(r, index) in recipe.extendedIngredients.split(',')"
                :key="index + '_' + r">
                {{ r }}
              </li> -->
            </ul>
          </div>
          <div class="wrapped">
              <div class="nutrition_signs">
                <!-- <div class="favorite_button" @click="FavoriteChange"> 
                <img v-if="!flag" src="../assets/not_favorite_icon.png" width="25" height="25" >
                <img v-if="flag" src="../assets/favorite_icon.png" width="25" height="25" >
                </div> -->
                  <img v-if="recipe.vegetarian" src="../assets/vegetarian_icon.png" width="25" height="25" >
                  <img v-if="recipe.vegan" src="../assets/vegan_icon.png" width="25" height="25" >
                  <img v-if="recipe.glutenFree" src="../assets/gluten_free_icon.png" width="25" height="25" >
              </div>
            <br>
            Instructions:
            <!-- <ol>
              <li v-for="(s, index) in recipe.analyzedInstructions.split(',')" :key="index + '_' + s">
                {{ s }}
              </li>
            </ol> -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null,
      flag: false
    };
  },
  async created() {
    try {
      let recipe_id = this.$route.params.recipeId
      //       console.log("hrre");

      // console.log(recipe_id);
      // console.log("hrre");
      let response;
      try {
        response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/ExpandeRecipeData?recipeID=" + recipe_id, { withCredentials: true });
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      let {
        analyzedInstructions,
        extendedIngredients,
        aggregateLikes,
        readyInMinutes,
        image,
        title,
        vegan,
        vegetarian,
        glutenFree,
        servings
      } = response.data;
        let _recipe = {
        analyzedInstructions,
        extendedIngredients,
        aggregateLikes,
        readyInMinutes,
        image,
        title,
        vegan,
        vegetarian,
        glutenFree,
        servings
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
h1{
    text-align:center; 
    color: rgb(182, 99, 22);
    font-family:'Gill Sans', 'Gill Sans MT';
    background-color: rgb(245, 234, 212);
    padding: 20px;
}

.favorite_button
{
  cursor: pointer;
}

</style>
