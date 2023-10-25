<script setup>
  import { supabase } from '../components/lib/supabaseClient'

  async function registerUser(userEmail, userPassword, accountType, userName, firstName, lastName) {
    const { data, error } = await supabase.auth.signUp({
          email: userEmail,
          password: userPassword,
          options: { 
              data: {
                  usertype: accountType, 
                  username: userName, 
                  firstname: firstName, 
                  lastname: lastName, 
                  email: userEmail,
              }
          }
      })
      };

  const formData = {
    username: '',
    email: '',
    password: '',
    last: '',
    first: '',
    accountType: '',
    errors: {
      username: '',
      email: '',
      password: '',
    },
  };


  const submitForm = () => {
    const numberValue = Number(formData.accountType) + 1;
    registerUser(formData.email.toString(), formData.password.toString(), numberValue, formData.username.toString(), formData.first.toString(), formData.last.toString());

  };
</script>

<template>
  <div class = "page">
    <div class="login">
      <h2>Register</h2>
      <form v-on:submit="submitForm">
        <div class="form-group">
          <label for="username">Username:</label>
          <input type="text" id="username" v-model="formData.username" />
          <span class="error">{{ formData.errors.username }}</span>
        </div>
        <div class="form-group">
          <label for="first">First Name:</label>
          <input type="text" id="first" v-model="formData.first" />
          <span class="error">{{ formData.errors.username }}</span>
        </div>
        <div class="form-group">
          <label for="last">Last Name:</label>
          <input type="text" id="last" v-model="formData.last" />
          <span class="error">{{ formData.errors.username }}</span>
        </div>
        <div class="form-group">
          <label for="email">Email:</label>
          <input type="email" id="email" v-model="formData.email" />
          <span class="error">{{ formData.errors.email }}</span>
        </div>
        <div class="form-group">
          <label for="password">Password:</label>
          <input type="password" id="password" v-model="formData.password" />
          <span class="error">{{ formData.errors.password }}</span>
        </div>
        <div class="form-group">
          <label for="toggle">Drone Owner?:</label>
          <div class="toggle-button">
            <input type="checkbox" id="toggle" v-model="formData.accountType" class="hidden-input" />
            <label for="toggle" class="toggle"></label>
          </div>
        </div>
        <div class = "spacer">
          <button type = "submit" formaction = "/">Cancel</button>
          <button 
          style = "background-color: var(--accent-color); color: var(--font-accent);"
          type="submit">
            Register
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
