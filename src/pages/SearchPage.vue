<template>
  <div class="container">
    <h1 class="title">Search for our recipes..</h1>

  </div>
</template>

<script>
// import {
//   required,
//   alpha,
// } from "vuelidate/lib/validators";

export default {
  name: "Register",
  data() {
    return {
      search_results: [],
      cuisine_options: [],
      diet_options: [],
      intolerance_options: [],
      // form: {
      //   query: "",
      //   cuisine: null,
      //   diet: null,
      //   intolerance: null,
      //   submitError: undefined
      // },
      // errors: [],
      // validated: false
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
  // validations: {
  //   form: {
  //     query: {
  //       required,
  //       alpha
  //     },
  //     cuisine: {
  //       required
  //     }
  //   }
  // },
  // methods: {
  //   validateState(param) {
  //     const { $dirty, $error } = this.$v.form[param];
  //     return $dirty ? !$error : null;
  //   },
  // }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 500px;
}
h1{
    text-align:center; 
    color: rgb(182, 99, 22);
    font-family:'Gill Sans', 'Gill Sans MT';
    background-color: rgb(245, 234, 212);
    padding: 20px;
}

</style>
