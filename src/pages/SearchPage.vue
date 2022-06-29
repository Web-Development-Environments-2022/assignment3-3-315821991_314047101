<template>
  <div class="container">
    <h1 class="title">Search for our recipes..</h1>

      <div class="input-query">
        <input size="50" type="text" placeholder="What would you like to search?" v-model="LastQuery" required @keyup="SearchAction">
      </div>
      <div v-if="!this.is_query" class="error_search">
        <p>You must eneter a query..</p>
      </div>
      
      <div class="search_filters">
        <!-- <p>How many results would you like to recieve?:</p> -->
        <b-form-select class="options" v-model="form.results_number" :options="number_options"></b-form-select><br>
        <!-- <p>Filter results by:</p>
        <p>cuisine:</p> -->
        <b-form-select class="options" v-model="form.cuisine" :options="cuisine_options"></b-form-select><br>
        <!-- <p>diet:</p> -->
        <b-form-select class="options" v-model="form.diet" :options="diet_options"></b-form-select><br>
        <!-- <p>intolerance:</p> -->
        <b-form-select class="options" v-model="form.intolerance" :options="intolerance_options"></b-form-select>
      </div>
      <br>

      <b-button class="reset_button" variant="outline-info" @click="onReset">Reset Filters</b-button>
      <div class="search_button" @click="onSearch"> <img src="../assets/search_icon.png" width="25" height="25" ></div>

  </div>
</template>

<script>
// import {
//   required,
//   alpha,
// } from "vuelidate/lib/validators";

export default {
  name: "Search",
  data() {
    return {
      search_results: [],
      cuisine_options: [],
      diet_options: [],
      number_options: [5, 10, 15],
      intolerance_options: [],
      form: {
        results_number: 5,
        cuisine: "",
        diet: "",
        intolerance: "",
        submitError: undefined
      },
      LastQuery: sessionStorage.getItem("LastQuery"),
      is_query: true,
    };
  },
  async created() {
      try {
        let response;
        response = await this.axios.get(
        this.$root.store.server_domain + "/recipes/search_filtering_options", { withCredentials: true });
        if (response.status !== 200) this.$router.replace("/NotFound");
        this.cuisine_options = response.data[0][0].split(",");
        this.diet_options = response.data[1][0].split(",");
        this.intolerance_options = response.data[2][0].split(",");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async onSearch() {
      let response;
      if(this.LastQuery == null || this.LastQuery == undefined || this.LastQuery == "")
      {
        this.is_query = false;
        return;
      }
      this.is_query = true;
      try {
        response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/search?query=" + this.LastQuery + "&number=" + this.form.results_number
          + "&cuisine='" + this.form.cuisine + "'&diet=" + this.form.diet + "&intolerance=" + this.form.intolerance, { withCredentials: true });
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      console.log(response)
    },
    onReset() {
        this.form.results_number = 5;
        this.form.cuisine = "";
        this.form.diet = "";
        this.form.intolerance = "";
    },
    SearchAction()
    {
      if(!this.is_query)
      {
        if(this.LastQuery != "")
        {
          this.is_query = true;
        }
      }
    }
    }
};
</script>
<style lang="scss" scoped>
.container {
  margin: 0 auto;
  width: 60%;
  text-align: center;

}

.input-query
{
  margin: 30px auto;
  display: flex;
  justify-content: center;
}
h1{
    text-align:center; 
    color: rgb(182, 99, 22);
    font-family:'Gill Sans', 'Gill Sans MT';
    background-color: rgb(245, 234, 212);
    padding: 20px;
}

.search_button
{
  display: flex;
  justify-content: center;
  margin: 0 0 20px;
  cursor: pointer;
}

.search_button:hover
{
  transform: translateY(-2px);
}

.error_search
{
  color: red;
}
.options
{
  justify-content: center;
  display: flex;
  width: 40%
}

.search_filters
{
  justify-content: center;
  display: flex;
  flex-direction: row;
}

.title
{
  margin: 0 0 20px;
}

.reset_button
{
    margin: 0 0 20px;
}

.reset_button:hover
{
  transform: translateY(-2px);
}

</style>
