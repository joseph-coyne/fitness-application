<template>
  <div class="users-edit">
    <!-- WRAPPER -->
    <div id="wrapper">

      <!-- PAGE TITLE -->
      <header id="page-title"> <!-- style="background-image:url('assets/images/demo/parallax_bg.jpg')" -->
        <!--
          Enable only if bright background image used
          <span class="overlay"></span>
        -->

        <div class="container">
          <h1>Edit Profile</h1>

          <ul class="breadcrumb">
            <li><router-link to="/trainers/me">Profile</router-link></li>
            <li class="active">Edit</li>
          </ul>
        </div>
      </header>

      <section id="contact" class="container">

        <div class="row">

          <!-- FORM -->
          <div class="col-md-8">

            <form id="contactForm" class="white-row" v-on:submit.prevent="submit()">
              <div class="row">
                <div class="form-group">
                  <div class="col-md-5">
                    <label>First Name</label>
                    <input type="text" v-model="firstName" maxlength="100" class="form-control" id="contact_name">
                  </div>                  
                  <div class="col-md-5">
                    <label>Last Name</label>
                    <input type="text" v-model="lastName" maxlength="100" class="form-control" id="contact_name">
                  </div>            
                  <div class="col-md-5">
                    <label>Username</label>
                    <input type="text" v-model="username" maxlength="100" class="form-control" id="contact_name">
                  </div>
                  <div class="col-md-5">
                    <label>E-mail</label>
                    <input type="email" v-model="email" data-msg-required="Please enter your email address." data-msg-email="Please enter a valid email address." maxlength="100" class="form-control"  id="contact_email">
                  </div>
                  <div class="col-md-5">
                    <label>Password</label>
                    <input type="password" v-model="password" data-msg-required="Please enter your phone" data-msg-email="Please enter your phone" maxlength="100" class="form-control"  id="contact_phone">
                  </div>          
                  <div class="col-md-5" v-if="password">
                    <label>Password Confirmation</label>
                    <input type="password" v-model="passwordConfirmation" data-msg-required="Please enter your phone" data-msg-email="Please enter your phone" maxlength="100" class="form-control" id="contact_phone">
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="form-group">
                  <div class="col-md-12">
                    <label>Profile Picture</label>
                    <input type="file" class="form-control" v-on:change="setFile($event)" ref="fileInput">
                  </div>
                </div>
              </div>
            <br />

              <div class="row">
                <div class="col-md-12">

                  <input id="contact_submit" type="submit" value="Save" class="btn btn-primary btn-lg" data-loading-text="Loading...">
                  <button id="trainerdelete" type="button" class="btn btn-danger" v-on:click="deleteUser(user)">Delete Account</button>
                </div>
              </div>
            </form>
          
          </div>
          <!-- /FORM -->


          <!-- INFO -->
          <div class="col-md-4">
            <div class="white-row">
              <figure>
                <img class="radius6 img-responsive" :src="user.image_url" alt="" />
              </figure>
            </div>
          </div>
          <!-- /INFO -->
        </div>
      </section>
    </div>
    <!-- /WRAPPER -->

  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      firstName: "",
      lastName: "",
      username: "",
      password: "",
      passwordConfirmation: "",
      email: "",
      image: "",
      errors: []
    };
  },
  created: function() {
    axios
      .get("http://localhost:3000/api/users/" + this.$route.params.id)
      .then(response => {
        this.user = response.data;
        this.firstName = response.data.first_name
        this.lastName = response.data.last_name
        this.username = response.data.username
        this.email = response.data.email
        this.height = response.data.height
        this.weight = response.data.weight
        this.image = response.data.image
      });
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    },
    submit: function() {
      var formData = new FormData();
        formData.append("first_name", this.firstName);
        formData.append("last_name", this.lastName);
        formData.append("username", this.username);
        formData.append("password", this.password);
        formData.append("password_confirmation", this.password_confirmation);
        formData.append("email", this.email);
        formData.append("image", this.image);
        formData.append("height", this.height);
        formData.append("weight", this.weight);
      axios
        .patch("http://localhost:3000/api/users/" + this.$route.params.id, formData)
        .then(response => {
          this.$refs.fileInput.value = "";
          this.$router.push("/users/me");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    deleteUser: function(user) {
      axios
        .delete("http://localhost:3000/api/users/" + this.$route.params.id)
        .then(response => {
          delete axios.defaults.headers.common["Authorization"];
          localStorage.removeItem("jwt");
          localStorage.removeItem("user_id");
          localStorage.removeItem("trainer_id");
          this.$router.push("/");
        });
    }
  }
};
</script>