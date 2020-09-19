<template>
    <div>
        <h1>Painel Admin</h1>
        <table class="table">
            <thead>
                <tr>
                    <th>Nome</th>
                    <th>Email</th>
                    <th>Cargo</th>
                    <th>Ações</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="user in users" :key="user.id">
                    <td>{{user.name}}</td>
                    <td>{{user.email}}</td>
                    <td>{{user.role | processRole}}</td>
                    <td>
                        <router-link :to="{name: 'UserEdit', params:{id: user.id}}"><button class="button is-success">Editar</button></router-link>
                        <button class="button is-danger" @click="showModalUser(user.id)">Deletar</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <div :class="{modal: true, 'is-active': showModal}">
            <div class="modal-background">
                <div class="card">
                    <header class="card-header">
                        <p class="card-header-title">
                            Você quer realmente deletar esse usuário?
                        </p>
                    </header>
                    <footer class="card-footer">
                        <a href="#" class="card-footer-item" @click="hideModal()">Cancelar</a>
                        <a href="#" class="card-footer-item" @click="deleteUser()">Sim, quero deletar!</a>
                    </footer>
                </div>
            </div>
            <div class="modal-content">
                
            </div>
            <button class="modal-close is-large" aria-label="close" @click="hideModal()">

            </button>
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

        axios.get("http://localhost:3000/user", req).then(res => {
            console.log(res);
            this.users = res.data;
        }).catch(err => {
            console.log(err);
        });
    },

    data(){
        return{
            users: [],
            showModal: false,
            deleteUserId: -1
        }
    },
    filters: {
        processRole: function(value){
            if(value == 0){
                return "Comum";
            }else if(value == 1){
                return "Administrador"
            }
        }
    },
    methods: {
        hideModal(){
            this.showModal = false;
        },
        showModalUser(id){
            this.deleteUserId = id;
            this.showModal = true;
        },
        deleteUser(){

            const req = {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('token') 
                }
            }


            axios.delete("http://localhost:3000/user/"+ this.deleteUserId, req).then(res => {
                console.log(res);
                this.showModal = false;
                document.location.reload(true);

            }).catch(err => {
                console.log(err);
                this.showModal = false;
            })
        }
    }
}
</script>

<style scoped>

</style>