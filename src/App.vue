<template>
  <div>
    <div class="container">
      <h3>Movies:</h3>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Id</th>
            <th scope="col">Name</th>
            <th scope="col">Email</th>
            <th scope="col"></th>
            <th scope="col"><button @click="mode = 'Add'">AddMovie</button></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="movie in movies" v-bind:key="movie.id">
            <th scope="row">{{ movie.id }}</th>
            <td>{{ movie.name }}</td>
            <td>{{ movie.productionYear }}</td>
            <td>
              <button
                @click="
                  mode = 'Delete';
                  target_movie = movie;
                "
              >
                Delete
              </button>
            </td>
            <td>
              <button
                @click="
                  mode = 'Edit';
                  target_movie = movie;
                  validate_movie.Name = movie.name;
                  validate_movie.ProductionYear = movie.productionYear;
                "
              >
                Edit
              </button>
            </td>
            <td>
              <button
                @click="
                  mode = 'Preview';
                  target_movie = movie;
                "
              >
                Preview
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div id="modal" v-if="mode != null">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ mode }}</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              @click="mode = null"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div v-if="mode == 'Preview'">
              <table class="table">
                <thead>
                  <tr>
                    <th scope="col">Id</th>
                    <th scope="col">Name</th>
                    <th scope="col">Production Year</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <th scope="row">{{ target_movie.id }}</th>
                    <td>{{ target_movie.name }}</td>
                    <td>{{ target_movie.productionYear }}</td>
                  </tr>
                  <tr>
                    <form>
                      <th scope="col"><input type="text" name="id" />ss</th>
                      <td scope="col"><input type="text" name="name" />ss</td>
                      <td scope="col">
                        <input type="text" name="productionYear" />ee
                      </td>
                    </form>
                  </tr>
                </tbody>
              </table>
            </div>

            <div v-if="mode == 'Delete'">
              Are you sure you wanna delete this movie ?
              <div class="modal-footer">
                <button
                  @click="deleteMovie(target_movie)"
                  type="button"
                  class="btn btn-primary"
                >
                  Yes, delete
                </button>
                <button
                  @click="mode = null"
                  type="button"
                  class="btn btn-secondary"
                >
                  Cancel
                </button>
              </div>
            </div>

            <div v-if="mode == 'Edit'">
              <form @submit.prevent="handleSubmit">
                <table class="table">
                  <thead>
                    <tr>
                      <th scope="col">Id</th>
                      <th scope="col">Name</th>
                      <th scope="col">Production Year</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td scope="col">
                        <input
                          size="10"
                          type="text"
                          name="id"
                          :value="target_movie.id"
                          disabled
                        />
                      </td>
                      <td scope="col">
                        <input
                          size="10"
                          type="text"
                          v-model="validate_movie.Name"
                          id="name"
                          name="name"
                          class="form-control"
                          :class="{
                            'is-invalid':
                              submitted && $v.validate_movie.Name.$error,
                          }"
                        />
                      </td>
                      <td scope="col">
                        <input
                          size="10"
                          type="text"
                          v-model="validate_movie.ProductionYear"
                          id="productionYear"
                          name="productionYear"
                          class="form-control"
                          :class="{
                            'is-invalid':
                              submitted &&
                              $v.validate_movie.ProductionYear.$error,
                          }"
                        />
                      </td>
                    </tr>
                  </tbody>
                </table>
                <button type="submit" class="btn btn-primary">
                  Submit Movie
                </button>
                <div
                  v-if="submitted && !$v.validate_movie.Name.required"
                  class="invalid-validate"
                >
                  Movie Name is required
                </div>
                <div
                  v-if="submitted && !$v.validate_movie.Name.movieLength"
                  class="invalid-validate"
                >
                  Minimum length of 3 required
                </div>

                <div
                  v-if="submitted && !$v.validate_movie.ProductionYear.required"
                  class="invalid-validate"
                >
                  Production Year is required
                </div>
                <div
                  v-if="
                    submitted && !$v.validate_movie.ProductionYear.yearRange
                  "
                  class="invalid-validate"
                >
                  Production Year should be between 1900 and 2100
                </div>
              </form>
            </div>

            <div v-if="mode == 'Add'">
              <form @submit.prevent="handleSubmit">
                <table class="table">
                  <thead>
                    <tr>
                      <th scope="col">Id</th>
                      <th scope="col">Name</th>
                      <th scope="col">Production Year</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td scope="col">
                        <input
                          size="10"
                          type="text"
                          name="id"
                          value="0"
                          disabled
                        />
                      </td>
                      <td scope="col">
                        <input
                          size="10"
                          type="text"
                          v-model="validate_movie.Name"
                          id="name"
                          name="name"
                          class="form-control"
                          :class="{
                            'is-invalid':
                              submitted && $v.validate_movie.Name.$error,
                          }"
                        />
                      </td>
                      <td scope="col">
                        <input
                          size="10"
                          type="text"
                          v-model="validate_movie.ProductionYear"
                          id="productionYear"
                          name="productionYear"
                          class="form-control"
                          :class="{
                            'is-invalid':
                              submitted &&
                              $v.validate_movie.ProductionYear.$error,
                          }"
                        />
                      </td>
                    </tr>
                  </tbody>
                </table>
                <button type="submit" class="btn btn-primary">
                  Submit Movie
                </button>
                <div
                  v-if="submitted && !$v.validate_movie.Name.required"
                  class="invalid-validate"
                >
                  Movie Name is required
                </div>
                <div
                  v-if="submitted && !$v.validate_movie.Name.movieLength"
                  class="invalid-validate"
                >
                  Minimum length of 3 required
                </div>

                <div
                  v-if="submitted && !$v.validate_movie.ProductionYear.required"
                  class="invalid-validate"
                >
                  Production Year is required
                </div>
                <div
                  v-if="
                    submitted && !$v.validate_movie.ProductionYear.yearRange
                  "
                  class="invalid-validate"
                >
                  Production Year should be between 1900 and 2100
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { between, required, minLength } from "vuelidate/lib/validators";
import Vue from "vue";
import Vuelidate from "vuelidate";
Vue.use(Vuelidate);

var urlAPI = "http://localhost:8081/api/movie";

export default {
  name: "MoviesItem",
  data() {
    return {
      movies: null,
      mode: null,
      target_movie: null,
      validate_movie: {
        Id: 0,
        Name: "",
        ProductionYear: 0,
      },
      submitted: false,
    };
  },
  validations: {
    validate_movie: {
      Id: { required },
      Name: { required, movieLength: minLength(3) },
      ProductionYear: { required, yearRange: between(1900, 2100) },
    },
  },

  created: function () {
    axios
      .get(urlAPI)
      .then((res) => {
        this.movies = res.data;
      })
      .catch(function (error) {
        alert("Error connecting with server check logs");
        if (error.response) {
          // The request was made and the server responded with a status code
          // that falls out of the range of 2xx
          console.log(error.response.data);
          console.log(error.response.status);
          console.log(error.response.headers);
        } else if (error.request) {
          // The request was made but no response was received
          // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
          // http.ClientRequest in node.js
          console.log(error.request);
        } else {
          // Something happened in setting up the request that triggered an Error 
          console.log("Error", error.message);
        }
        console.log(error.config);
      });
  },

  methods: {
    deleteMovie(movie) {
      axios
        .delete(urlAPI + "/" + movie.id)
        .then(() => {
          this.mode = null;
          const index = this.movies.indexOf(movie);
          if (index > -1) {
            this.movies.splice(index, 1); // 2nd parameter means remove one item only
          }
        })
        .catch(function (error) {
          alert("Error connecting with server check logs");
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            console.log(error.response.data);
            console.log(error.response.status);
            console.log(error.response.headers);
          } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request);
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log("Error", error.message);
          }
          console.log(error.config);
        });
    },
    addMovie(movie) {
      axios
        .post(urlAPI, {
          Id: 0,
          Name: movie.Name,
          ProductionYear: parseInt(movie.ProductionYear),
        })
        .then((res) => {
          this.movies.push(res.data);
        })
        .catch(function (error) {
          alert("Error connecting with server check logs");
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            console.log(error.response.data);
            console.log(error.response.status);
            console.log(error.response.headers);
          } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request);
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log("Error", error.message);
          }
          console.log(error.config);
        });
    },
    editMovie(movie) {
      movie = {
        Id: parseInt(this.target_movie.id),
        Name: movie.Name,
        ProductionYear: parseInt(movie.ProductionYear),
      };
      axios
        .put(urlAPI + "/" + this.target_movie.id, movie)
        .then((res) => {
          if (res.data == "") {
            const index = this.movies.indexOf(this.target_movie);
            if (index > -1) {
              this.movies[index].name = movie.Name;
              this.movies[index].productionYear = movie.ProductionYear;
            }
          }
        })
        .catch(function (error) {
          alert("Error connecting with server check logs");
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            console.log(error.response.data);
            console.log(error.response.status);
            console.log(error.response.headers);
          } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request);
          } else {
            // Something happened in setting up the request that triggered an Error
            console.log("Error", error.message);
          }
          console.log(error.config);
        });
    },
    handleSubmit() {
      this.submitted = true;
      // stop here if form is invalid

      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }
      if (this.mode == "Add") this.addMovie(this.validate_movie);
      if (this.mode == "Edit") this.editMovie(this.validate_movie);
      this.mode = null;
    },
  },
};
</script>
<style>
#modal {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>