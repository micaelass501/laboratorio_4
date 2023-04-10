<template>
    <div style="text-align: center">
        <h1>Editar Estudiante</h1>        
        <div class="container text-left">
            <div class="row">
              <div class="col">
                <div class="mb-3">
                    <label for="formGroupExampleInput" class="form-label">Nombre completo:</label>
                    <input type="text" v-model="payload.nombre" class="form-control"  placeholder="nombre completo">
                  </div>
                <div class="mb-3">
                    <label for="formGroupExampleInput2" class="form-label">CI:</label>
                    <input type="text" v-model="payload.ci" class="form-control" placeholder="carnet de identidad">
                </div>
                <div class="mb-3">
                    <label for="formGroupExampleInput2" class="form-label">Matricula:</label>
                    <input type="text" v-model="payload.matricula" class="form-control" placeholder="matricula">
                </div>
                <div class="mb-3">
                    <label for="formGroupExampleInput2" class="form-label">Dirección:</label>
                    <input type="text" v-model="payload.direccion" class="form-control" placeholder="dirección">
                </div>
                <div class="mb-3">
                    <button type="button" class="btn btn-primary" @click="editarDatosEstudiante()">Editar datos personales</button>
                </div>
              </div>
              <div class="col">
                <table class="table">
                    <thead>
                      <tr>
                        <th scope="col">#</th>
                        <th scope="col">Materia</th>
                        <th scope="col">Calificación</th>
                        <th scope="col">Acción</th>
                      </tr>
                    </thead>
                    <tbody>
                        <tr>
                          <td>1</td>
                          <td>Matematica</td>
                          <td >{{verCalificacion('Matematica')}}</td>  
                          <td><input type="text" v-model="notaMatematica"  class="form-control"></td> 
                          <td><button type="button" class="btn btn-primary" @click="editarNota('Matematica',notaMatematica)">Editar</button></td>
                        </tr>
                        <tr>
                          <td>2</td>
                          <td>Fisica</td>
                          <td >{{verCalificacion('Fisica')}}</td>  
                          <td><input type="text" v-model="notaFisica" class="form-control"></td> 
                          <td><button type="button" class="btn btn-primary" @click="editarNota('Fisica',notaFisica)">Editar</button></td>
                        </tr>
                        <tr>
                          <td>3</td>
                          <td>Quimica</td>
                          <td >{{verCalificacion('Quimica')}}</td>  
                          <td><input type="text" v-model="notaQuimica" class="form-control"></td> 
                          <td><button type="button" class="btn btn-primary" @click="editarNota('Quimica',notaQuimica)">Editar</button></td>
                        </tr>
                      </tbody>
                  </table>
              </div>
            </div>
        </div>
    </div>    
</template>

<script>
    export default{
        name:'EstudianteEditarView',
        props:[],
        emits:[],
        data() {
            return {
                estudianteId:0,
                Calificaciones:[],
                MisCalificaciones:[],
                nota:0,
                payload:{
                    nombre: "",
                    ci: 0,
                    matricula: "",
                    direccion: ""
                },
                notaMatematica:0,
                notaQuimica:0,
                notaFisica:0,
                CalificacionEdit: {
                    id_estudiante: 0,
                    materia: "",
                    nota: 0
                }

            }
        },
        methods: {


getData(){

   // this.notaMatematica = this.verCalificacion('Matematica');
        //       this.notaQuimica:0,
          //     this.notaFisica:0,

},

            load(){
                this.estudianteId = this.$route.params.id;                
            },
            getEstudiante(){                
                this.axios.get("http://localhost:5000/Estudiante/"+this.estudianteId)
                .then((response)=>{ this.payload = response.data;})
                .catch((error)=>{console.log(error);})
            },
            editarDatosEstudiante(){                
                this.axios.patch("http://localhost:5000/Estudiante/"+this.estudianteId,this.payload)
                .then((response)=>{ console.log(response);})
                .catch((error)=>{console.log(error);})
            },
            
            getCalificaciones(){
                this.axios.get("http://localhost:5000/Calificaciones/")
                .then((response)=>{ this.Calificaciones = response.data;})
                .catch((error)=>{console.log(error);})
            },
            verCalificacion(materia){  
                let notasw = 0;
                let swid = this.payload.id;
                this.Calificaciones.forEach(function(calificacion) {                    
                    if (calificacion.id_estudiante === swid && calificacion.materia === materia) {
                        notasw = calificacion.nota;
                    }
                });
                return notasw;
            },
            editarNota(materia,nota){
console.log(materia,nota);

                let id_califica = 0;
                let swid = this.payload.id;
                this.Calificaciones.forEach(function(calificacion) {                    
                    if (calificacion.id_estudiante === swid && calificacion.materia === materia) {
                        id_califica = calificacion.id;
                    }
                });
                this.CalificacionEdit.id_estudiante=swid;
                this.CalificacionEdit.materia=materia;
                this.CalificacionEdit.nota=parseInt(nota);

                this.axios.patch("http://localhost:5000/Calificaciones/"+id_califica,this.CalificacionEdit)
                .then((response)=>{ console.log(response);})
                .catch((error)=>{console.log(error);})
            },
            // editarNota(item){
            //     console.log(item);

            //     this.CalificacionEdit.id_estudiante=item.id_estudiante;
            //     this.CalificacionEdit.materia=item.materia;
            //     this.CalificacionEdit.nota=parseInt(item.nota);

            //     this.axios.patch("http://localhost:5000/Calificaciones/"+item.id,this.CalificacionEdit)
            //     .then((response)=>{ console.log(response);})
            //     .catch((error)=>{console.log(error);})
            // },
            verMisCalificacion(){
                var sw = this.estudianteId;
                var resp=[];
                console.log("this.Calificaciones ", this.Calificaciones);
                this.Calificaciones.forEach(function(calificacion) {                  
                    if (calificacion.id_estudiante === sw) {
                        resp.push(calificacion);
                    }
                });
                console.log("resp ", resp);
                this.MisCalificaciones =resp; 
            }
        },
        computed:{

        },
        mounted() {
            this.load();
            this.getEstudiante();
            this.getCalificaciones();
                // console.log("resp ", this.Calificaciones);
                // this.verMisCalificacion();
        },
        components:{

        }
    }
</script>
  