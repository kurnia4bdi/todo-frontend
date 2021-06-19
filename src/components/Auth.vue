<template>
    <div>
        <div>
            <label for="username">username </label>
            <input type="text" v-model="username" />
        </div>
        <div>
            <label for="password">password </label>
            <input type="password" v-model="password" />
        </div>
        <button @click="simpan">Save</button>
        <button @click="hapus">Hapus</button>
    </div>
</template>

<script>
export default {
    data: function () {
        return {
            username: '',
            password: ''
        }
    },
    methods: {
        simpan: function () { 
            localStorage.setItem('usr', this.username)
            localStorage.setItem('pwd', this.password)
         },
        hapus: function () {
            localStorage.removeItem('usr')
            localStorage.removeItem('pwd')
        }
    }
}
</script>

<style>

</style>