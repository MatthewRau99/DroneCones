<script setup>
  import { supabase } from '../components/lib/supabaseClient';
  import { getUser } from '~/database/userDatabase';

  const formData = {
    email: '',
    password: ''
  };
  async function redirectToProfilePage(datas) {
    try {
      const data = await getUser(datas.user.id); 
      if (data !== null) {
        console.log(data.usertype);
        if(data.usertype == 1) {
          window.location.href = '/order';
        }
        else if(data.usertype == 2) {
          window.location.href = '/droneHome';
        }
        else {
          window.location.href = '/managerHome';
        }
        } else {
          console.log("User not found");
        }
    } catch (error) {
      console.error("Error:", error);
    }   
  }

  function navigateToRegister(){
    window.location.href = '/register'
  }
    //Write a function that validates login, and forwards to corresponding page. 
 
  async function attemptLogin() {
    
    console.log(formData.email);
    
    const { data, error } = await supabase.auth.signInWithPassword({
      email: formData.email,
      password: formData.password, 
    })

    console.log("asdf")
    if (error) {
      console.error('Login error:', error.message);
    } else {
      console.log('Logged in as', data); 
      localStorage.setItem('userID', data.user.id);
      console.log(data.user.id);
      redirectToProfilePage(data);         // Redirect or perform actions after a successful login
    }
  }
</script>
<template>
  <div class="page">
    <h1>Drone Cones</h1>
    <div class="login">
      <h2>Login</h2>
      <form @submit.prevent="attemptLogin">
        <div class="form-group">
          <label for="email">Email:</label>
          <input type="email" id="email" v-model="formData.email" />
        </div>
        <div class="form-group">
          <label for="password">Password:</label>
          <input style="font-weight: 900;" type="password" id="password" v-model="formData.password" />
        </div>
        <div class="spacer">
          <button
            type = "submit"
            style="background-color: var(--accent-color); color: var(--font-accent)"
          >
            Login
          </button>
          <button @click="navigateToRegister">Register</button>
        </div>
      </form>
    </div>
  </div>
</template>



  