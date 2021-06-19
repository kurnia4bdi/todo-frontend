<template>
  <div>
    <div>
      <h1>Berikut adalah daftar tugas kita</h1>
    </div>
    <ul>
      <li v-for="item in todos" :key="item.id">{{item.deskripsi}} <button @click="hapus(item.id)"> X </button></li>
    </ul>
    <input v-model="myText" />
    <button @click="tambah">Add</button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data : function() {
    return {
      todos : [],
      myText : '',
      loUsername : localStorage.getItem('usr'),
      loPassword : localStorage.getItem('pwd')
    }
  },
  created: function() {
    axios.get('http://localhost:3080/todo', {headers: {username:this.loUsername, password:this.loPassword}} )
    .then((result)=>{
      this.todos = result.data
    })
  },
  methods : {
    tambah: function () { 
      let newItem = {deskripsi: this.myText} 
      axios.post('http://localhost:3080/todo', newItem, {headers: {username:this.loUsername, password:this.loPassword}})
      .then((response) => {
        console.log(newItem)
        this.todos.push({id:response.data.id, deskripsi:this.myText})
      })
    },
    hapus: function (id) {
      axios.delete(`http://localhost:3080/todo/${id}`, {headers: {username:this.loUsername, password:this.loPassword}})
      .then(()=>{
        var filtered = this.todos.filter((item) => item.id != id)
        this.todos = filtered
      })
    }
  }
}
</script>
