<template>
  <div id="formulario-blog">
    <form @submit.prevent="enviarFormulario">
      <div class="container">
        <div class="row">
          <div class="col-md-6">
            <div class="form-group">
              <label>Nombre de la pelicula</label>
              <input
                ref="name"
                v-model="blog.name"
                type="text"
                class="form-control"
                :class="{'is-invalid' : procesando && nameInvalido}"
                @focus="resetEstado"
                @keypress="resetEstado"
                
              />
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label>Descripción</label>
              <input
                v-model="blog.description"
                type="text"
                :class="{'is-invalid' : procesando && descriptionInvalido}"
                class="form-control"
                @focus="resetEstado"
              />
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label>Duración</label>
              <input
                v-model="blog.duration"
                type="text"
                :class="{'is-invalid' : procesando && durationInvalido}"
                class="form-control"
                @focus="resetEstado"
              />
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label>Calificación</label>
              <input
                v-model="blog.calification"
                type="text"
                :class="{'is-invalid' : procesando && calificationInvalido}"
                class="form-control"
                @focus="resetEstado"
              />
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-6 m-2">
            <div class="form-group">
              <button class="btn btn-primary">Añadir blog</button>
            </div>
          </div>
        </div>
      </div>
      <div class="container">
        <div class="row">
          <div class="col-md-12">
            <div
              v-if="error && procesando"
              class="alert alert-danger"
              role="alert"
            >
              !Debes llenar todos los campos!
            </div>
            <div v-if="correcto" class="alert alert-success" role="alert">
              ¡El blog ha sido agregado correctamente!
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "formulario-blog",
  data() {
    return {
      procesando: false,
      correcto: false,
      error: false,
      blog: {
        name: "",
        description: "",
        duration: "",
        calification: "",
      },
    };
  },
  methods: {
    enviarFormulario() {
      this.procesando = true;
      this.resetEstado();
      //Comprobamos si existen errores
        if(this.nameInvalido || this.descriptionInvalido || this.durationInvalido || this.calificationInvalido){
            this.error = true
            return
        }
      //Enviamos el método
      this.$emit("crear-blog", this.blog);
      this.$refs.name.focus();
      this.error = false;
      this.correcto = true;
      this.procesando = false;
      //Limpiamos los valores del input
      this.blog = {
        name: "",
        description: "",
        duration: "",
        calification: "",
      };
    },
    resetEstado() {
      this.correcto = false;
      this.error = false;
    },
  },
  computed: {
    nameInvalido() {
      return this.blog.name.length < 1;
    },
    descriptionInvalido() {
      return this.blog.description.length < 1;
    },
    durationInvalido() {
      return this.blog.duration.length < 1;
    },
    calificationInvalido() {
      return this.blog.calification.length < 1;
    },
  },
};
</script>