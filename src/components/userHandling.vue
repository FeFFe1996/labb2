<script setup>
import { ref} from 'vue'
import booklist from './booklist.vue'

const usernameInput = ref('')
const passwordInput = ref('')
const currentUser = ref(null)
const errorMessage = ref('')

async function LoginUser() {
    errorMessage.value = ""
    try{
        const response = await fetch('./public/data/users.json')
        if(!response.ok){
            throw new Error("Error, couldnt connect to user database")
        }

        const data = await response.json();

        const checkUser = data.users.find(
            user => user.userName === usernameInput.value && user.passWord === passwordInput.value
        )

        if(!checkUser){
            throw new Error("Error, password or username i wrong. please try again")
        }

        currentUser.value = checkUser
        localStorage.setItem('user', JSON.stringify(checkUser))
    }catch(err){
        errorMessage.value = err.Message
    }
}

function logout() {
    currentUser.value = null
    localStorage.removeItem('user')
}

</script>

<template>
    <section v-if="!currentUser">
        <h3>Välkommen till min booklista</h3>
        <p style="width: 800px; margin: 0 auto;" >Lorem ipsum dolor sit amet consectetur, adipisicing elit. Corporis quae molestiae sed. Eaque cum eveniet ratione tempora et aspernatur minus, quaerat labore consequatur veritatis temporibus explicabo nisi ad dolores recusandae!</p>
        <h2>Login</h2>
        <form @submit.prevent="LoginUser">
            <input class="loginIn"
                v-model="usernameInput" 
                type="text" 
                placeholder="Username" 
                required 
            /><br>
            <input class="loginIn"
                v-model="passwordInput" 
                type="password" 
                placeholder="Password" 
                required 
            /><br>
            <button class="loginBtn" type="submit">Log In</button>
        </form>
        <p v-if="errorMessage" style="color: red;">{{ errorMessage }}</p>
    </section>
    <section v-else>
      <h2>Välkommen, {{ currentUser.userName }}!</h2>
      <booklist />
      <button class="logoutBtn" @click="logout">Log Out</button>
    </section>
</template>