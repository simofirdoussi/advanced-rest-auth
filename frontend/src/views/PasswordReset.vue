<template>

<div>
    <form @submit.prevent="resetInitiate()">
    <label for="email">Enter your email: </label>
    <input type="email" v-model="email" name="email" id="email">
    <button type="submit"> submit</button>
    </form>
</div>
    
</template>

<script>
import axios from 'axios'
export default {
    name:'PasswordReset',
    data() {
        return {
            email:'',
        }
    },
    methods:{
        resetInitiate(){
            var formData={
                email : this.email
            }
            axios 
            .post('http://127.0.0.1:8000/api/users/reset_password/', formData)
            .then(response => {
                console.log(response);
                this.$router.push('/')
            })
            .catch(error => {
                console.log(error);
            });
        }
    }
}
</script>