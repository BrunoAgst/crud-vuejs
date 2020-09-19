<template>
    <div>
        <h2>Login</h2>
        <hr>

        <div class="columns is-centered">
            <div class="column is-half">
                <div v-if="error != undefined">
                    <div class="notification is-danger">
                        {{error}}
                    </div>
                </div>
                  <label>E-mail</label>
                <input type="email" placeholder="E-mail do usuário" class="input" v-model="email"/>
                <label>Senha</label>
                <input type="password" placeholder="Senha do usuário" class="input" v-model="password"/>
                <button class="button is-success" @click="login">Login</button>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
export default {
    data(){
        return {
            email: '',
            password: '',
            error: undefined
        }
    },
    methods: { 
        login(){
             axios.post("http://localhost:3000/login", {
               password: this.password,
               email: this.email

            }).then(res =>{
                localStorage.setItem('token', res.data.token);
                //this.$router.push({name: 'Login'});
                return
            }).catch(error => {
                var msg = error.response.data.err;
                this.error = msg;
                return
            });
           
           
        }
    }
}
</script>

<style scoped>
    input{
        margin-bottom: 10px;
    }
    button{
        margin-top: 10px;
    }
</style>