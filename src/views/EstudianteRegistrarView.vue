<template>
    <div style="text-align: center">
        <h1>Registrar Estudiante</h1>        
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
                    <table class="table">
                        <thead>
                          <tr>
                            <th scope="col">#</th>
                            <th scope="col">Materia</th>
                            <th scope="col">Calificación</th>
                          </tr>
                        </thead>
                        <tbody>
                          <tr>
                            <td>1</td>
                            <td>Matematica</td>
                            <td><input type="text" v-model="notaMatematica" class="form-control"></td> 
                          </tr>
                          <tr>
                            <td>2</td>
                            <td>Fisica</td> 
                            <td><input type="text" v-model="notaFisica" class="form-control"></td> 
                           </tr>
                          <tr>
                            <td>3</td>
                            <td>Quimica</td>
                            <td><input type="text" v-model="notaQuimica" class="form-control"></td> 
                          </tr>
                        </tbody>
                      </table>
                </div>
                <div class="mb-3">
                    <button type="button" class="btn btn-primary" @click="registrarEstudiante()">Registrar estudiante</button>
                </div>
              </div>
              <div class="col">
                
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
                
                payload:{
                    nombre: "",
                    ci: 0,
                    matricula: "",
                    direccion: ""
                },
                
                    notaMatematica:0,
                    notaQuimica:0,
                    notaFisica:0,
                CalificacionReg: {
                    id_estudiante: 0,
                    materia: "",
                    nota: 0
                }

            }
        },
        methods: {

            registrarEstudiante(){
                let id_est =0;
                this.axios.post("http://localhost:5000/Estudiante",this.payload)
                .then((response)=>{
                  console.log(response); 
                  id_est = response.data.id;
                  var notaMat = this.notaMatematica;
                  this.registrarCalificacion(id_est,"Matematica", parseInt(notaMat));
                  var notaFis = this.notaFisica;
                  this.registrarCalificacion(id_est,"Fisica",parseInt(notaFis));
                  var notaQuim = this.notaQuimica;
                  this.registrarCalificacion(id_est,"Quimica",parseInt(notaQuim));
                })
                .catch((error)=>{console.log(error);});                
                console.log("id_est",id_est);
                
            },
            registrarCalificacion(id,materia,nota){
                
                this.CalificacionReg.id_estudiante = id;
                this.CalificacionReg.materia = materia;
                this.CalificacionReg.nota = nota;
                this.axios.post("http://localhost:5000/Calificaciones",this.CalificacionReg)
                .then((response)=>{console.log(response);})
                .catch((error)=>{console.log(error);});


            }
            
        },
        computed:{

        },
        mounted() {
        },
        components:{

        }
    }
</script>
  