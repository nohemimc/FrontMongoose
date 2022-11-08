<template>
    <div id="tabla-blog">
        <div v-if="!blogs.length" class="alert alert-info" role="alert">
            No hay blogs registrados
        </div>
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>Nombre</th>
                    <th>Descripción</th>
                    <th>Duración</th>
                    <th>Calificación</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="blog in blogs" :key="blog._id">

                    <td v-if="editando === blog._id">
                        <input type="text" class="form-control" v-model="blog.name">
                    </td>
                    <td v-else>
                        {{blog.name}}
                    </td>

                    <td v-if="editando === blog._id">
                        <input type="text" class="form-control" v-model="blog.description">
                    </td>
                    <td v-else>
                        {{blog.description}}
                    </td>

                    <td v-if="editando === blog._id">
                        <input type="text" class="form-control" v-model="blog.duration">
                    </td>
                    <td v-else>
                        {{blog.duration}}
                    </td>

                    <td v-if="editando === blog._id">
                        <input type="text" class="form-control" v-model="blog.calification">
                    </td>
                    <td v-else>
                        {{blog.calification}}
                    </td>

                    <td v-if="editando === blog._id">
                        <button class="btn btn-success" @click="guardarBlog(blog)">💾 Guardar</button>
                        <button class="btn btn-secondary ml-2" @click="cancelarEdicion(blog)">❌ Cancelar</button>
                    </td>
                    <td v-else>
                        <button class="btn btn-danger" @click="$emit('eliminar-blog',blog)">🗑️ Borrar</button>
                        <button class="btn btn-info ml-2" @click="editarBlog(blog)">✏️ Editar</button>
                    </td>

                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default{
        name: 'tabla-blog',
        props:{
            blogs: {},
        },
        data(){
            return{
                editando : null,
            }
        },
        methods: {
            editarBlog(blog){
                this.blogEditado = Object.assign({},blog)
                this.editando = blog._id
            },
            guardarBlog(blog){
                // if(!blog.name.length || !blog.description.length || !blog.duration.length || !blog.calification.length){
                //     return
                // }
                this.$emit('actualizar-blog',blog)
                this.editando = null
            },
            cancelarEdicion(blog){
                Object.assign(blog,this.blogEditado)
                this.editando = null
            }
        },
    }
</script>