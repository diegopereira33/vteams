<template>
  <div class="customers container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Manage Teams</h1>
    <input class="form-control" placeholder="Name" v-model="filterInput">
    <br />
    <table class="table table-striped">
        <thead>
          <tr>
            <th>Id</th>
            <th>Name</th>
            <th>Description</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="customer in filterBy(customers, filterInput)">
            <td>{{customer.id}}</td>
            <td>{{customer.name}}</td>
            <td>{{customer.description}}</td>
            <td><router-link class="btn btn-default" v-bind:to="'/customer/'+customer.id">View</router-link></td>
          </tr>
        </tbody>
    </table>
  </div>
</template>

<script>
  import Alert from './Alert';
  export default {
    name: 'customers',
    data () {
      return {
        customers: {},
        alert:'',
        filterInput:''
      }
    },
    methods: {
      fetchCustomers(){
        this.$http.get('http://localhost:5000/api/teams')
          .then(function(response){
            //this.customers = response.body;
              this.customers = response.body;
          });
      },
      filterBy(list, value){
        value = value.charAt(0).toUpperCase() + value.slice(1);
        return list.filter(function(customer){
          //return customer.last_name.indexOf(value) > -1;
            return customer.name.indexOf(value) > -1;
        });
      }
    },
    created: function(){
      if(this.$route.query.alert){
        this.alert = this.$route.query.alert;
      }
      this.fetchCustomers();
    },
    updated: function(){
      this.fetchCustomers();
    },
    components: {
      Alert
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
