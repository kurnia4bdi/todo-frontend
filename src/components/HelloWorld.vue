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
  created: function() {
    axios.get('http://localhost:3080/todo')
    .then((result)=>{
      this.todos = result.data
    })
  },
  data : function() {
    return {
      todos : [],
      myText : '',
      lastId : null
    }
  },
  methods : {
    tambah: function () { 
      let newItem = {deskripsi: this.myText} 
      axios.post('http://localhost:3080/todo', newItem)
      .then(() => {
        this.todos.push(newItem)
        // location.reload()
      })
    },
    hapus: function (id) {
      // alert(id)
      axios.delete(`http://localhost:3080/todo/${id}`)
      .then(()=>{
        var filtered = this.todos.filter((item) => item.id != id)
        this.todos = filtered
      })
    }
  }
}
</script>
