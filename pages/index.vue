<template>
  <section class="section">
      <div class="columns is-desktop is-vcentered">
        <div>
          <b-field label="Username">
              <b-input v-model="username" maxlength="30"></b-input>
          </b-field>

          <b-field label="Password">
              <b-input v-model="password" type="password" maxlength="30"></b-input>
          </b-field>

          <b-button type="is-primary" @click="login">Login</b-button>
        </div>
      </div>
  </section>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: 'HomePage',
  
  data() {
    return {
      username: "",
      password: ""
    }
  },
  methods: {
    async login() {
      const credentials = {
        identifier: this.username,
        password: this.password
      };
      try {
        const { data: { login: { user, jwt } } } = await this.$apollo.mutate({
          mutation: gql`
            mutation($identifier: String!, $password: String!) {
              login(input: { identifier: $identifier, password: $password }) {
                user {
                  id
                  username
                  email
                  role {
                    name
                    type
                    description
                  }
                }
                jwt
              }
            }
          `,
          variables: credentials
        })
        //set the jwt to the this.$apolloHelpers.onLogin
        await this.$apolloHelpers.onLogin(jwt)
        console.log(jwt)
      } 
      
      catch (e) {
        console.log(credentials)
        console.error(e)
      }

      this.$router.push('/dashboard')
    }
  },
  async mounted(){
    //clear apollo-token from cookies to make sure user is fully logged out
    await this.$apolloHelpers.onLogout() 
  }
}
</script>
