<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <h2 class="subTitles">
      <RecipePreviewList title="Explore this recipes" class="RandomRecipes center" />
    </h2>
    
   <div>
    <h2 class="subTitles">
      <h1   class="title">Last Viewed Recipes</h1>
      <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to view this</router-link>
       <b-container v-else>
                <div v-if="recipes.length==0" class="center_div">
                    You have not yet viewed any recipes on our website...
                </div>
              <b-row >
                <b-col v-for="r in recipes" :key="r.id" >
                  <RecipePreview  style="width: 270px; height:273px" class="recipePreview" :recipe="r" />
                </b-col>
              </b-row>
      </b-container>
    </h2>
   </div>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import RecipePreview from "../components/RecipePreview.vue";
export default {
  components: {
    RecipePreviewList,
    RecipePreview
   },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.LastThreeViewes();
  },
  methods: {
    async LastThreeViewes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/getThreeLastViewedRecipes", { withCredentials: true }
        );
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;

}
h1
{
    text-align:center; 
    color: rgb(182, 99, 22);
    font-family:'Gill Sans', 'Gill Sans MT';
    background-color: rgb(245, 234, 212);
    padding: 20px;
}
h2
{
    text-align:center; 
    color: rgb(182, 99, 22);
    font-family:'Gill Sans', 'Gill Sans MT';
    background-color: rgb(245, 234, 212);
    padding: 20px;
}

.center_div {
  display: flex;
  justify-content: center;
  font-size: 20px;
}
</style>
