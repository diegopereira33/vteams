<template>
  <div class="details container">
    <router-link to="/">Back</router-link>
    <h1 class="page-header">{{customer.first_name}} {{customer.last_name}}
        <span class="pull-right">
            <router-link class="btn btn-primary" v-bind:to="'/edit/'+customer.id">Edit</router-link>
            <button class="btn btn-danger" v-on:click="deleteCustomer(customer.id)">Delete</button>
            </span>
    </h1>
<br>

        <ul class="list-group">
            <li class="list-group-item"> {{customer.id}}</li>
            <li class="list-group-item">{{customer.name}}</li>
            <li class="list-group-item">{{customer.description}}</li>
        </ul>
  </div>
</template>

<script>
export default {
  name: 'customerdetails',
  data () {
    return {
      customer: ''
    }
  },
  methods:{
      fetchCustomer(id){
          this.$http.get('http://localhost:5000/api/teams/'+id)
          .then(function(response){
            this.customer = response.body;
          });
      },
      deleteCustomer(id){
          this.$http.delete('http://localhost:5000/api/teams/'+id)
          .then(function(response){
            this.$router.push({path: '/', query: {alert: 'Team Deleted'}});
          });
      }
  },
  created: function(){
      this.fetchCustomer(this.$route.params.id);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
