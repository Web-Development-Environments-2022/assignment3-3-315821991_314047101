<template>
  <div class="container">
    <h1 class="title">Search for our recipes..</h1>
    <b-form @submit.prevent="onRegister">
      <b-form-group
        id="input-group-lastname"
        label-cols-sm="3"
        label="Last Name:"
        label-for="lastname"
      >
        <b-form-input
          id="lastname"
          v-model="$v.form.lastname.$model"
          type="text"
          :state="validateState('lastname')"
        >
        </b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.lastname.required">
          Last Name is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.lastname.alpha">
          Last Name must contain only letters
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group id="input-group-cuisine" label-cols-sm="3" label="Cuisine:" label-for="cuisine">
        <b-form-select id="cuisine" v-model="$v.form.cuisine.$model" :options="cuisine_options" ></b-form-select>
      </b-form-group>

      <b-form-group id="input-group-diet" label-cols-sm="3" label="Diet:" label-for="diet">
        <b-form-select id="diet" v-model="$v.form.cuisine.$model" :options="diet_options" ></b-form-select>
      </b-form-group>

      <b-form-group id="input-group-intolerance" label-cols-sm="3" label="Intolerance:" label-for="intolerance">
        <b-form-select id="intolerance" v-model="$v.form.cuisine.$model" :options="intolerance_options" ></b-form-select>
      </b-form-group>

      <b-button
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Register</b-button>

    </b-form>

  </div>
</template>

<script>
import {
  required,
  alpha,
} from "vuelidate/lib/validators";

export default {
  name: "Register",
  data() {
    return {
      search_results: [],
      cuisine_options: [],
      diet_options: [],
      intolerance_options: [],
      form: {
        lastname: "",
        cuisine: null,
        diet: null,
        intolerance: null,
        password: "",
        submitError: undefined
      },
      errors: [],
      validated: false
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
  validations: {
    form: {
      lastname: {
        required,
        alpha
      },
      cuisine: {
        required
      }
    }
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Register() {
      try {
        const response = await this.axios.post(
          this.$root.store.server_domain + "/Register",

          {
            lastname: this.form.lastname,
            password: this.form.password,
            cuisine: this.form.cuisine,
            diet: this.form.diet,
            intolerance: this.form.intolerance,
          }
        );
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onRegister() {
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      this.Register();
    }
  }
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
