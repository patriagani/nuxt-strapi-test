<template>
  <div class="container is-fullhd" style="display:flex; align-items:center; justify-content:center; height:100vh; background-color:#7957D5;">
    <div class="box" style="width:500px;">
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
      <br>
      <h5 v-if="user.company">Country Code: {{user.company.countryCode}}</h5>
      <br>
      <h5 v-if="user.company">Timezone: {{user.company.timezone}}</h5>
      <br>
      <h5 v-if="user.company">Contact Number: {{user.company.contactNumbers[0].contactNumber}}</h5>
      <br>
      <br>
      <b-button type="is-primary" @click="logout">Sign-out</b-button>
    </div>
  </div>
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
              countryCode
              timezone
              contactNumbers {
                contactNumber
              }
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
  methods: {
    logout() {
      this.$router.push('/')
    }
  }

}
</script>
