<template>

  <section class="src-componentes-notas">
   <div class="jumbotron">
      <h2><i>Ingreso de Notas</i></h2>
      <hr>
      <br>

      <vue-form :state="formstate" @submit.prevent="enviar()">
            
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input 
            type="text"
            id="nombre" 
            v-model="formData.nombre" 
            required name="nombre" 
            autocomplete="off" 
            class="form-control"
            :minlength="nombreMinLength"
            :maxlength="nombreMaxLength"
          />
    
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo requiere como mínimo {{nombreMinLength}} caracteres.
            </div>
            <div slot="maxlength" class="alert alert-danger mt-1">
              {{nombreMaxLength}} es la cantidad maxima de caracteres permitidos.
            </div>
          </field-messages>
        </validate>
        <br>

        <validate tag="div">
          <label for="apellido">Apellido</label>
          <input 
            type="text"
            id="apellido" 
            v-model="formData.apellido" 
            required name="apellido" 
            autocomplete="off" 
            class="form-control"
            :minlength="nombreMinLength"
            :maxlength="nombreMaxLength"
          />
    
          <field-messages name="apellido" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo requiere como mínimo {{nombreMinLength}} caracteres.
            </div>
            <div slot="maxlength" class="alert alert-danger mt-1">
              {{nombreMaxLength}} es la cantidad maxima de caracteres permitidos.
            </div>
          </field-messages>
        </validate>
        <br>

        <validate tag="div">
          <label for="nota">Nota</label>
          <input 
            type="number"
            id="nota"
            name="nota" 
            class="form-control"
            autocomplete="off"
            v-model.number="formData.nota" 
            required 
            :min="notaMin"
            :max="notaMax"
          />

          <field-messages name="nota" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            <div slot="min" class="alert alert-danger mt-1">
              La nota mínima permitida es de {{notaMin}} años.
            </div>
            <div slot="max" class="alert alert-danger mt-1">
              La nota máxima permitida es de {{notaMax}} años.
            </div>
          </field-messages>
        </validate>
        <br>

        <button class="btn btn-success my-4" :disabled="formState.$invalid" @click="enviar()">Enviar</button>
        
      </vue-form>
      <br>
      <hr>

      <h2><i>Historial de Notas</i></h2>

      <div v-if="alumnos.lenghth" class="table-responsive">
        <table class="table table-dark">
            <tr>
                <th>ALUMNO</th>
                <th>NOTA</th>
            </tr>
            <tr v-for="(alumno,index) in alumnos" :key="index" :style="{color: analizarNota(alumno).color}">
                <td>{{ alumno.nombre }} {{ alumno.apellido }}</td>
                <td>{{ analizarNota(alumno).nota }}</td>
            </tr>
            <tr :style="{color: calcularPromedio().color}">
              <td>{{calcularPromedio().mensaje}}</td>
              <td>{{calcularPromedio().promedio}}</td>
            </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-info">No hay notas ingresadas</h3>
   </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-notas',
    props: [],
    mounted () {

    },
    data () {
      return {
        formstate :{},
        formData :this.getInitialData(),
        nombreMinLength: 3,
        nombreMaxLength: 15,
        notaMin: 0,
        notaMax:10,
        alumnos: [],

      }
    },
    methods: {
        getInitialData() {
          return {
            nombre : null,
            apellido: null,
            nota: null,
          }
        },

        enviar() {
          this.alumnos.push({...this.formData})
          this.formData = this.getInitialData(),
          this.formState._reset()

        },

        analizarNota(alumno) {
          let nota = alumno.nota
          let color = '#F00'
          if(nota > 3) color ='#FF0'
          if(nota > 6) color ='#0F0'
          return{
            nota : nota,
            color
          }

        }

    },
    computed: {
      calcularPromedio(){
        let cant = this.alumnos.length
        let total = 0
        let mensaje = 'PROMEDIO TOTAL'
        for(alumno in alumnos) {
          total += alumno.nota
        }
        let promedio = total/cant
        let color = '#F00'
          if(nota > 3) color ='#FF0'
          if(nota > 6) color ='#0F0'
        return {
          mensaje : mensaje,
          promedio : promedio,
          color

        }
          


      }

    }
}


</script>

<style scoped lang="css">
  .src-componentes-notas {

  }

  .jumbotron {
    background-color: rgb(115, 115, 220);
    color: white;
  }
  hr {
    background-color: #bbb;
  }
</style>
