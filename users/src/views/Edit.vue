<template>
    <div>
        <h2>Editar usuário</h2>
        <hr>

        <div class="columns is-centered">
            <div class="column is-half">
                <div v-if="error != undefined">
                    <div class="notification is-danger">
                        {{error}}
                    </div>
                </div>
                <label>Nome</label>
                <input type="text" placeholder="Nome do usuário" class="input" v-model="name"/>
                <label>E-mail</label>
                <input type="email" placeholder="E-mail do usuário" class="input" v-model="email"/>
                <button class="button is-success" @click="update">Editar</button>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
export default {
    created(){
        const req = {
            headers: {
                Authorization: "Bearer " + localStorage.getItem('token') 
            }
        }

        axios.get("http://localhost:3000/user/" + this.$route.params.id, req).then(res => {
            console.log(res);
            this.name = res.data.name;
            this.email = res.data.email;
            this.id = res.data.id;
        }).catch(err => {
            this.$router.push({name: 'Users'});
            console.log(err);
        })
    },
    data(){
        return {
            name: '',
            email: '',
            id: -1,
            error: undefined
        }
    },
    methods: { 
        update(){
            const req = {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('token') 
                }
            }
            axios.put("http://localhost:3000/user", {
               id: this.id,
               name: this.name,
               email: this.email

            }, req).then(() =>{
                this.$router.push({name: 'Users'});
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