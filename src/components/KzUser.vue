<template>
	<div class="container mt-4">
        <div class="row justify-content-center">
          <div class="col-lg-3 col-12 mb-lg-0 mb-3">
            <h2 class="text-dark text-center py-3">KZ-user Save</h2>
            <form @submit.prevent="save" class="p-4 border-rounded">
              <div class="mb-3">
                <label for="exampleInputEmail1" class="form-label">Name</label>
                <input type="text" class="form-control" id="exampleInputEmail1" v-model="user.name">
              </div>
              <div class="mb-3">
                <label for="exampleInputPassword1" class="form-label">Designation</label>
                <input type="text" class="form-control" id="exampleInputPassword1" v-model="user.designation">
              </div>
              <div class="mb-3">
                <label for="exampleInputPassword2" class="form-label">Position</label>
                <input type="text" class="form-control" id="exampleInputPassword2" v-model="user.position">
              </div>
              <button type="submit" class="btn btn-primary w-100 mt-2">Submit</button>
            </form>
          </div>
          <div class="col-lg-8 col-12">
            <h2 class="font-weight-bold text-dark text-center py-3">KZ-user View</h2>
            <table class="table table-striped">
              <thead>
                <tr>
                  <th scope="col">ID</th>
                  <th scope="col">User Name</th>
                  <th scope="col">Designation</th>
                  <th scope="col">Position</th>
                  <th scope="col">Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="kzuser in result" v-bind:key="kzuser.id">   
                  <td>{{ kzuser.id }}</td>
                  <td>{{ kzuser.name }}</td>
                  <td>{{ kzuser.designation }}</td>
                  <td>{{ kzuser.position }}</td>
                  <td>
                  	<div class="d-flex justify-content-between">
                  		<button type="button" class="btn btn-success btn-sm" @click="editUser(kzuser)">Edit</button>
                  		<button type="button" class="btn btn-danger btn-sm" @click="deleteUser(kzuser)">Delete</button>
                  	</div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
      </div>
    </div>
</template>


<style scoped>
   table, form{
      border: 5px solid rgb(109, 226, 93);
   }

   thead tr th, tbody tr td{
      text-align: center;
   }
</style>

<script>

import Vue from 'vue';
import axios from 'axios';

//Vue.use(axios)
window.axios = require('axios');

export default {
  name: 'KzUserView',
  data () {
      return {
        result: {},
        user: {
          id : '',
          name : '',
          designation : '',
          position : '',
        }
      }
  },
  created() {
    this.loadUser();
  },
  methods: {
    loadUser()
    {
      var page = 'http://127.0.0.1:8000/api/vuejsdata';
      axios.get(page)
      .then(
        ({data}) => {
          this.result = data;
        }
      );
    },
    save(){
      if(this.user.id == ''){
         this.saveUser();
      }
      else{
         this.updateUser();
      }
    },
    saveUser(){
      axios.post('http://127.0.0.1:8000/api/vuejsdataSave',this.user)
      .then(
        ({data})=>{
          alert('Saved !!');
          this.loadUser();
        }
      )
    },
    editUser(user){
    	this.user = user;
    },
    updateUser(){
    	var updateUser = 'http://127.0.0.1:8000/api/vuejsdataUpdate/'+this.user.id;
    	axios.put(updateUser, this.user)
    	.then(
    		({data})=>{
    			this.user.name = '',
    			this.user.designation = '',
    			this.user.position = '',
    			this.user.id = ''
    			alert('Updated !!');
    			this.loadUser();
    		}
    	);
    },
    deleteUser(user){
      var url = `http://127.0.0.1:8000/api/vuejsdataDelete/${user.id}`;
      axios.delete(url);
      alert("Deleted !!");
      this.loadUser();
    }
  }
}
</script>