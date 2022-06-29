<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <h2 class="subTitles">
      <h1   class="title">Explore this recipes</h1>
      <RecipePreviewList class="RandomRecipes center" />
    </h2>
    
   <div>
    <h5 class="subTitles">
      <h1   class="title"><br>Last Viewed Recipes</h1>
      <router-link class="need_login" v-if="!$root.store.username" to="/login" tag="button">Login to view this page</router-link>
       <b-container v-else>
                <div v-if="recipes.length==0" class="center_div">
                    You have not yet viewed any recipes on our website...
                </div>
                 <b-col >
              <b-row v-for="r in recipes" :key="r.id" >
               
                  <RecipePreview  style="width: 400px; height:315px" class="recipePreview" :recipe="r" />
                </b-row>
                </b-col>
              
      </b-container>
    </h5>
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
h5
{
position:absolute;
  left: 760px;
  right: 180px;
  top:220px;
  height: 1307px;

  background-color: rgb(245, 234, 212);
}
.RandomRecipes {
  margin: 10px 0 50px;
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
    padding: 50px;
    margin: 20px 0 10px;
    height: 1300px;
    width: 550px;
    top:900px;

}

.need_login{
  background-color: rgb(182, 99, 22);
  border-color: gray;
  color: white;
  box-shadow: 0 10px 15px 0 rgba(0,0,0,0.24), 0 30px 50px 0 rgba(0,0,0,0.19);
  font-size: 20px;
}

.need_login:hover
{
  transform: translateY(2px);
  box-shadow: 0 30px 30px 0 rgba(0,0,0,0.24), 0 30px 50px 0 rgba(0,0,0,0.19);
}

.center_div {
  display: flex;
  justify-content: center;
  font-size: 20px;
}
</style>
