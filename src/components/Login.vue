<template>
   <div class="login-form mt-5">
  <div v-show="error" class="alert alert-danger">
    {{ errorMsg }}
  </div>
  <div v-show="success" class="alert alert-success">
    {{ successMsg }}
  </div>
  <h2 class="login-form__title">Login Account</h2>
  <form class="login-form__form" @submit.prevent="loginUser">
    <div class="login-form__input-group">
      <label for="email" class="login-form__label">Email:</label>
      <input type="email" id="email" class="login-form__input" v-model="email">
    </div>
    <div class="login-form__input-group">
      <label for="password" class="login-form__label">Password:</label>
      <input type="password" id="password" class="login-form__input" v-model="password">
    </div>
    <div class="login-form__submit">
      <button class="login-form__button" type="submit">Login</button>
    </div>
    <div class="login-form__register-link">
      <router-link to="register" style="color:black;">Don't have an account yet?</router-link>
    </div>
  </form>
  <div class="login-form__providers">
    <div class="login-form__providers-label">Or sign in with:</div>
    <div class="login-form__providers-buttons">
      <button type="button" class="provider-btn provider-google" @click.prevent="signInWithGoogle">
        <i class="fab fa-google provider-icon"></i>
        <span class="provider-label">Google</span>
      </button>
      <button type="button" class="provider-btn provider-facebook" @click.prevent="signInWithFacebook">
        <i class="fab fa-facebook provider-icon"></i>
        <span class="provider-label">Facebook</span>
      </button>
      <button type="button" class="provider-btn provider-twitter" @click.prevent="signInWithTwitter">
        <i class="fab fa-twitter provider-icon"></i>
        <span class="provider-label">Twitter</span>
      </button>
      <button type="button" class="provider-btn provider-yahoo" @click.prevent="signInWithYahoo">
        <i class="fab fa-yahoo provider-icon"></i>
        <span class="provider-label">Yahoo</span>
      </button>
    </div>
  </div>
</div>
  </template>

<script setup>
  import { ref } from 'vue'
  import { auth } from '../Firebase/index.js'
  import { GoogleAuthProvider, signInWithEmailAndPassword, signInWithPopup, FacebookAuthProvider, TwitterAuthProvider } from '@firebase/auth'
  import router from '../router/index.js'

  const email = ref('')
  const password = ref('')
  const error = ref('')
  const errorMsg = ref('')
  const success = ref('')
  const successMsg = ref('')

  const loginUser = () => {
    if (email.value && password.value) {
      signInWithEmailAndPassword(auth, email.value, password.value)
        .then((cred) => {
          if (cred.user.emailVerified) {
            router.push('/home')
          } else {
            error.value = true
            errorMsg.value = "Your account is not yet verified."
            email.value = ''
            password.value = ''
          }
        })
        .catch((err) => {
          error.value = true
          errorMsg.value = err.message
        })
    } else {
      error.value = true
      errorMsg.value = 'Please fill out both email and password fields.'
    }
  }

  const signInWithGoogle = () => {
    const provider = new GoogleAuthProvider()
    provider.setCustomParameters({ prompt: 'select_account' })
    signInWithPopup(auth, provider)
      .then((cred) => {
        console.log(cred.user)
        router.push('/dashboard')
      })
      .catch((err) => {
        error.value = true
        errorMsg.value = err.message
      })
  }

  const signInWithFacebook = () => {
    const provider = new FacebookAuthProvider()
    signInWithPopup(auth, provider)
      .then((cred) => {
        console.log(cred.user)
        router.push('/dashboard')
      })
      .catch((err) => {
        error.value = true
        errorMsg.value = err.message
      })
  }

  const signInWithTwitter = () => {
    const provider = new TwitterAuthProvider()
    signInWithPopup(auth, provider)
      .then((cred) => {
        console.log(cred.user)
        router.push('/dashboard')
      })
      .catch((err) => {
        error.value = true
        errorMsg.value = err.message
      })
  }

  const signInWithYahoo = () => {
    const provider = new YahooAuthProvider()
    signInWithPopup(auth, provider)
      .then((cred) => {
        console.log(cred.user)
        router.push('/dashboard')
      })
      .catch((err) => {
        error.value = true
        errorMsg.value = err.message
      })
  }

</script>

<style scoped>
.login-form {
  max-width: 500px;
  margin: 0 auto;
  padding: 1.5rem;
  border: 2px solid #030303;
  border-radius: 0.5rem;
}

.login-form__title {
  text-align: center;
  margin-bottom: 1.5rem;
}

.login-form__form {
  display: flex;
  flex-direction: column;
}

.login-form__input-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}

.login-form__label {
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.login-form__input {
  padding: 0.5rem;
  border-radius: 0.5rem;
  border: 1px solid #ccc;
}

.login-form__submit {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
}

.login-form__button {
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: #fff;
  border-radius: 0.5rem;
  border: none;
  cursor: pointer;
}

.provider-btn {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  background-color: #FFFFFF;
  border: 2px solid #BBBBBB;
  border-radius: 4px;
  color: #333333;
  display: inline-block;
  font-size: 14px;
  font-weight: bold;
  margin-right: 10px;
  padding: 10px 10px;
  margin-top: 3px;
  text-transform: uppercase;
  width: 200px;
  box-sizing: border-box;
}

.provider-btn:hover {
  background-color: #3b3939;
  cursor: pointer;
}

.provider-google {
  border-color: green;
  background-color: green;
}

.provider-facebook {
  border-color: #3B5998;
  background-color: #3B5998;
}

.provider-twitter {
  border-color: #1DA1F2;
  background-color: #1DA1F2;
}

.provider-yahoo {
  border-color: #6C0089;
  background-color: #6C0089;
}

.provider-icon {
  margin-right: 10px;
  color: white;
}

.provider-label {
  vertical-align: middle;
  color: white;
}


.login-form {
text-align: center;
}

.login-form__title {
font-size: 24px;
font-weight: bold;
margin-bottom: 20px;
}

.login-form__form {
margin-bottom: 20px;
}

.login-form__input-group {
display: flex;
flex-direction: column;
margin-bottom: 10px;
text-align: left;
}

.login-form__label {
font-size: 16px;
font-weight: bold;
margin-bottom: 5px;
}

.login-form__input {
border: 1px solid #ccc;
border-radius: 4px;
font-size: 16px;
padding: 8px;
}

.login-form__submit {
display: flex;
justify-content: center;
margin-bottom: 20px;
}

.login-form__button {
background-color: #007bff;
border: none;
border-radius: 4px;
color: #fff;
font-size: 16px;
font-weight: bold;
padding: 10px 20px;
text-transform: uppercase;
}

</style>