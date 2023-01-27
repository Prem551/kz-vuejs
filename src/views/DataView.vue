<template>
    <div class="container">
        <div class="row justify-content-center">
          <div class="col-lg-8 col-12">
            <h2 class="font-weight-bold text-dark py-3">KZ-user View</h2>
            <table class="table table-striped">
              <thead>
                <tr>
                  <th scope="col">ID</th>
                  <th scope="col">User Name</th>
                  <th scope="col">Destination</th>
                  <th scope="col">Position</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="kzuser in result" v-bind:key="kzuser.id">   
                  <td>{{ kzuser.id }}</td>
                  <td>{{ kzuser.name }}</td>
                  <td>{{ kzuser.designation }}</td>
                  <td>{{ kzuser.position }}</td>
                </tr>
              </tbody>
            </table>
          </div>
      </div>
    </div>
</template>

<style scoped>
   table{
      border: 5px solid rgb(109, 226, 93);
   }
</style>


<script>

import Vue from 'vue';
import axios from 'axios';

//Vue.use(axios)
window.axios = require('axios');

export default {
  name: 'DataView',
  data () {
      return {
        result: {},
      }
  },
  created() {
    this.loadtask();
  },
  methods: {
    loadtask()
    {
      var page = 'http://127.0.0.1:8000/api/vuejsdata';
      axios.get(page)
      .then(
        ({data}) => {
          this.result = data;
        }
      );
    }
  }
}
</script>