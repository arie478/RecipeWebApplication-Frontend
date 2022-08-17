<template>
  <div class="container">
    <b-card border-variant="secondary" header-border-variant="secondary" header="Login" align="center">
      <b-card-text>

        <b-form @submit.prevent="onLogin">

          <b-form-group id="input-group-Username" label-cols-sm="3" label="Username:" label-for="Username">
            <b-form-input id="Username" v-model="$v.form.username.$model" type="text"
              :state="validateState('username')" />
            <b-form-invalid-feedback>Username is required</b-form-invalid-feedback>
          </b-form-group>

          <b-form-group id="input-group-Password" label-cols-sm="3" label="Password:" label-for="Password">
            <b-form-input id="Password" type="password" v-model="$v.form.password.$model"
              :state="validateState('password')"></b-form-input>
            <b-form-invalid-feedback>Password is required</b-form-invalid-feedback>
          </b-form-group>

          <b-button type="submit" variant="primary" style="width:330px; display:block;">Login</b-button>
          <div class="mt-2">Do not have an account yet?<router-link to="register"> Register in here</router-link>
          </div>
        </b-form>

        <b-alert class="mt-2" v-if="form.submitError" variant="warning" dismissible show>Login failed: {{
            form.submitError
        }} </b-alert>

      </b-card-text>
    </b-card>
  </div>
</template>

<script>

import { required } from "vuelidate/lib/validators";

export default
  {
    name: "Login",

    data() {
      return {
        form:
        {
          username: "",
          password: "",
          submitError: undefined
        }
      };
    },

    validations:
    {
      form:
      {
        username: { required },
        password: { required }
      }
    },

    methods:
    {

      validateState(param) {
        const { $dirty, $error } = this.$v.form[param];
        return $dirty ? !$error : null;
      },

      onLogin() {
        console.log("onLogin() method called");
        this.form.submitError = undefined;
        this.$v.form.$touch();
        if (this.$v.form.$anyError) { return; }

        this.Login();
      },

      async Login() {
        try {
          this.axios.defaults.withCredentials = true;
          const response = await this.axios.post(
            // "http://localhost:3000/Login",
            "https://foodisgood.cs.bgu.ac.il/Login",
            {
              username: this.form.username,
              password: this.form.password
            }
          );
          this.axios.defaults.withCredentials = false;
          console.log(response);
          // this.$root.loggedIn = true;
          console.log(this.$root.store.login);
          this.$root.store.login(this.form.username);
          this.$router.push("/").catch(() => { this.$forceUpdate(); });
        }
        catch (err) {
          console.log(err.response);
          this.form.submitError = err.response.data.message;
        }
      }
    }
  };
</script>

<style lang="scss" scoped>
.container {
  max-width: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 1000px;
}
</style>
