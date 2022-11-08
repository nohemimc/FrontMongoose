<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12 mt-2">
        <h1>Blogs</h1>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <formulario @crear-blog="postData" />
        <tabla
          :blogs="blogs"
          @eliminar-blog="deleteData"
          @actualizar-blog="putData"
        ></tabla>
      </div>
    </div>
  </div>
</template>

<script>
import Tabla from './views/table.vue'
import Formulario from './views/form.vue'
import axios from 'axios'

  export default{
    name: 'app',
    components: {
      Tabla,
      Formulario,
    },
    data(){
      return{
        blogs: {},
      }
    },
    methods: {
      getData: async function() {
        try {
          const {data} = await axios.get('http://localhost:3000/api/blogs/')
          this.blogs = data.data
        } catch (error) {
          console.log(error)
        }
      },

      postData: async function(blog){
        try {
          const {data} = await axios({url: `http://localhost:3000/api/blogs/`, 
            method: 'post',
            headers:{ 'Content-Type': 'application/json'}, 
            data: JSON.stringify(blog)
          })
          const blogCreado = await data.data
          this.blogs = [...this.blogs , blogCreado]
        } catch (error) {
          console.log(error);
        }
      },
  
      putData: async function(blog){
        try {
          const {data} = await axios.put(`http://localhost:3000/api/blogs/${blog._id}`, {
            data: JSON.stringify(blog),
            headers: {'Content-type':'application/json; charset=UTF-8'},
          })
          const blogActualizado = await data.data
          this.blogs = this.blogs.map(u => (u._id === blog._id ? blogActualizado : u))
        } catch (error) {
          console.log(error);
        }
      },

      deleteData: async function(blog){
        try {
          await axios.delete(`http://localhost:3000/api/blogs/${blog._id}`)
          this.blogs = this.blogs.filter(u => u._id !== blog._id)
        } catch (error) {
          console.log(error);
        }
      }
    },
    mounted(){
      this.getData()
    },
  }
</script>
