<template>
  <div class="edit container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Edit Team</h1>
    <form v-on:submit="updateCustomer">
        <div class="well">
            <h4>Team Info</h4>
            <div class="form-group">
                <label>Id</label>
                <input type="text" class="form-control" placeholder="Id" v-model="customer.id">
            </div>
            <div class="form-group">
                <label>Name</label>
                <input type="text" class="form-control" placeholder="Name" v-model="customer.name">
            </div>

           <div class="form-group">
               <label>Description</label>
               <input type="text" class="form-control" placeholder="Description" v-model="customer.description">
            </div>
        </div>
        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
    import Alert from './Alert'
    export default {
    name: 'add',
    data () {
        return {
        customer: {},
        alert:''
        }
    },
    methods: {
        fetchCustomer(id){
            this.$http.get('http://localhost:5050/api/teams/'+id)
            .then(function(response){
                this.customer = response.body;
            });
        },
        updateCustomer(e){
            if(!this.customer.id || !this.customer.name || !this.customer.description){
                this.alert = 'Please fill in all required fields';
            } else {
                let updCustomer = {
                    id: this.customer.id,
                    name: this.customer.name,
                    description: this.customer.description
                }

                this.$http.put('http://localhost:5000/api/teams/'+this.$route.params.id, updCustomer)
                //this.$http.put('http://localhost:5000/api/teams/'+id, updCustomer)
                    .then(function(response){
                        this.$router.push({path: '/', query: {alert: 'Team Updated'}});
                    });

                e.preventDefault();
            }
            e.preventDefault();
        }
    },
    created: function(){
        this.fetchCustomer(this.$route.params.id);
    },
    components:{
        Alert
    }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
