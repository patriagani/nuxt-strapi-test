<template>
  <section class="section">
        <h2>Hi, {{user.username}}</h2>
        <br>
        <h4><strong>Account information</strong></h4>
        <br>
        <h5>Username: {{user.username}}</h5>
        <br>
        <h5>E-mail: {{user.email}}</h5>
        <br>
        <br>
        <h4><strong>Company information</strong></h4>
        <br>
        <h5 v-if="user.company">Company: {{user.company.name}}</h5>
        
  </section>
</template>

<script>
import gql from 'graphql-tag'

export default {
  name: 'Dashboard',
  middleware: ['authenticated'],
  
   data(){
    return {
      me: {},
      user:{}
    }
  },
  apollo: {
    me: {
      query: gql`
        query {
          me{
            id
            email
            username
            role{
              name
            }
          }
        }
      ` 
    },
    user: {
      query: gql`
        query($id: ID!) {
          user(id: $id){
            id
            email
            username
            company {
              name
            }
          }
        }
      ` ,
      variables() {
        return {
          id: this.me.id
        }
      }
    }
  },
  mounted() {
    
  }

}
</script>
