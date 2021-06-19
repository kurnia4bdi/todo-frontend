<template>
    <div>
        <div>
            <h1>Berikut adalah daftar tugas kita</h1>
        </div>
        <ul>
            <li v-for="item in todos" :key="item.id">
                {{ item.deskripsi }} <button @click="hapus(item.id)">X</button>
            </li>
        </ul>
        <input v-model="myText" />
        <button @click="tambah">Add</button>
    </div>
</template>

<script>
import axios from 'axios';
import io from 'socket.io-client';
export default {
    data: function () {
        return {
            todos: [],
            myText: '',
            loUsername: localStorage.getItem('usr'),
            loPassword: localStorage.getItem('pwd'),
            socket: io('http://localhost:3080', {
                transportOptions: ['websocket', 'polling'],
            }),
        };
    },
    created: function () {
        axios
            .get('http://localhost:3080/todo', {
                headers: {
                    username: this.loUsername,
                    password: this.loPassword,
                },
            })
            .then((result) => {
                this.todos = result.data;
            });
        this.socket.on('todo-add', (data) => {
            this.todos.push({
                id: data.data.insertedID,
                deskripsi: data.data.deskripsi,
            });
        });
        this.socket.on('todo-remove', (data) => {
            var filtered = this.todos.filter(
                (item) => item.id != data.data.deletedID
            );
            this.todos = filtered;
        });
    },
    methods: {
        tambah: function () {
            let newItem = { deskripsi: this.myText };
            this.socket.emit('todo-add', newItem);
        },
        hapus: function (id) {
            this.socket.emit('todo-remove', id);
        },
    },
};
</script>
