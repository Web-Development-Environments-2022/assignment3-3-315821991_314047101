<template>
  <router-link :to="{ name: path_name, params: { recipeId: recipe.id } }" class="recipe-preview">
     <div style="text-align:center"  class="recipe-body">  
       <img class="recipe_preview_img" style="width: 270px; height:173px;" :src="recipe.image"   />
           
    </div>
    <div style="text-align:center" class="recipe-footer">
      <div style="text-align:center" :title="recipe.title" class="recipe-title">
      {{ recipe.title }}
      </div>
      <ul class="recipe-overview">
        <li>{{ recipe.readyInMinutes }} minutes</li>
        <li>{{ recipe.aggregateLikes }} likes</li>
      </ul>
      <ul class="recipe-overview">
        <img v-if="!flag" class="favorite_button" src="../assets/not_favorite_icon.png" width="25" height="25" >
        <img v-if="flag" class="favorite_button" src="../assets/favorite_icon.png" width="25" height="25" >
        <img v-if="recipe.vegetarian" src="../assets/vegetarian_icon.png" width="25" height="25" >
        <img v-if="recipe.vegan" src="../assets/vegan_icon.png" width="25" height="25" >
        <img v-if="recipe.glutenFree" src="../assets/gluten_free_icon.png" width="25" height="25" >
      </ul>
    </div>
  </router-link>
</template>

<script>
export default {
  mounted() {
    //console.log(this.recipe.id);
    this.axios.get(this.recipe.image).then((i) => {
          this.image_load = true;
    });
  },
  data() {
    return {
      path_name:'recipe',
      image_load: false,
      flag: false
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true
    },
    //   methods: {
    // async FavoriteChange() {
    //   console.log(this.flag);
    //   // todo - need to finish
    // }
  //}
  },
  async created() {
    let recipe_id = this.recipe.id
    if(recipe_id < 0)
    {
      this.path_name = 'personal_recipe_view';
    }
    try {
      let favorite_response;
      try {
        favorite_response = await this.axios.get(
          this.$root.store.server_domain + "/users/get_favorites_ids", { withCredentials: true });
        if (favorite_response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.favorite_response.status", error.favorite_response.status);
        this.$router.replace("/NotFound");
        return;
      }
      for (let i = 0; i < favorite_response.data.length; i++) {
        if(favorite_response.data[i] == recipe_id)
        {
          this.flag = true;
        }
      }
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>

.recipe_preview_img:hover
{
  transform: translateY(2px);
  box-shadow: 0 30px 30px 0 rgba(0,0,0,0.24), 0 30px 50px 0 rgba(0,0,0,0.19);

}

.recipe_preview_img
{
  box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24), 0 17px 50px 0 rgba(0,0,0,0.19);
}

.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}
.recipe-image{
  width: 50%;
  height: 60px;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

li{
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 15pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.favorite_button
{
  cursor: pointer;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}
</style>
