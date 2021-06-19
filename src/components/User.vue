<template>
    <div>
        <div>
            <h1>Berikut adalah daftar user yang terdaftar</h1>
        </div>
        <ul>
            <li v-for="item in users" :key="item.id">
                {{ item.username }} <button @click="hapus(item.id)">X</button>
            </li>
        </ul>
        <input v-model="username" />
        &nbsp;
        <input v-model="password" />
        &nbsp;
        <button @click="tambah">Add</button>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data: function () {
        return {
            users: [],
            username: '',
            password: '',
            loUsername: localStorage.getItem('usr'),
            loPassword: localStorage.getItem('pwd'),
        };
    },
    created: function () {
        // let username = localStorage.getItem('usr')
        // let password = localStorage.getItem('pwd')
        axios
            .get('http://localhost:3080/user', {
                headers: {
                    username: this.loUsername,
                    password: this.loPassword,
                },
            })
            .then((result) => {
                this.users = result.data;
            });
    },
    methods: {
        tambah: function () {
            let newUser = { username: this.username, password: this.password };
            axios
                .post('http://localhost:3080/user', newUser, {
                    headers: {
                        username: this.loUsername,
                        password: this.loPassword,
                    },
                })
                .then((response) => {
                    this.users.push({
                        id: response.data.id,
                        username: this.username,
                        password: this.password,
                    });
                });
        },
        hapus: function (id) {
            axios
                .delete(`http://localhost:3080/user/${id}`, {
                    headers: {
                        username: this.loUsername,
                        password: this.loPassword,
                    },
                })
                .then(() => {
                    var filtered = this.users.filter((item) => item.id != id);
                    this.users = filtered;
                });
        },
    },
};
</script>
