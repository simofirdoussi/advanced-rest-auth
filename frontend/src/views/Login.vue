<template>
    <div>
        <h1>Login</h1>
        <form @submit.prevent="submitForm">
            <input type="text" v-model="email" placeholder='email'><br>
            <input type="password" v-model="password" placeholder='password'><br>
            <p v-for="error in errors" v-bind:key="error"> {{error}}</p>
            <button type="submit">Login</button>
        </form>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name:'Login',
    data(){
        return {
            email: '',
            password: '',
            errors: [],
        }
    },
    methods:{
        submitForm(){
            const formData = {
                email: this.email,
                password: this.password,
            }
            axios
                .post('http://127.0.0.1:8000/api/token/login/', formData)
                .then(response=>{
                    const token = response.data.auth_token
                    this.$store.commit('setToken', token) //calling the mutation
                    axios.defaults.headers.common['Authorization']='Token '+token //adding the token to the headers
                    localStorage.setItem('token', token) //setting the token to the localstorege, so that the user won't have to repeat everything if he/she reloads the page
                    this.$router.push('/signup')
                })
                .catch(error => { //catching errors
                    if (error.response) {
                        for (const property in error.response.data) {
                            this.errors.push(`${property}: ${error.response.data[property]}`)
                        }
                    } else {
                        this.errors.push('An error has occured, please try again')
                        
                        console.log(JSON.stringify(error))
                    }
                })
        }
    }
}
</script>