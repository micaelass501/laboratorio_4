<template>
    <div style="text-align: left">
        <h1>Lista Estudiantes</h1>
        <hr>
        <div class="container text-left">
            <div class="row">
              <div class="col">
                <button type="button" class="btn btn-success" @click="RegistrarEstudiante()">Registrar estudiante</button>
              </div>
              <div class="col">
                <div class="d-flex" role="search">
                    <input class="form-control me-2" type="search" v-model="nombreBusqueda" placeholder="nombre" aria-label="Search">
                    <button class="btn btn-outline-success" type="button" @click="BuscarEstudiante()">Buscar</button>
                </div>
              </div>
              <div class="col">
                <div class="d-flex" role="search">
                    <input class="form-control me-2" type="search" v-model="promedioMayor" placeholder="Promedio" aria-label="Search">
                    <button class="btn btn-outline-danger" type="button" @click="BuscarEstudiantePromedioMayor()">Promedio</button>
                </div>
              </div>
            </div>
        </div>
        <hr>
        <table class="table">
            <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Nombre completo</th>
                <th scope="col">Matematica</th>
                <th scope="col">Fisica</th>
                <th scope="col">Quimica</th>
                <th scope="col">Promedio</th>
                <th scope="col" colspan="2">Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(value,key) of Estudiantes" :key="key" id="key">
                <td>{{value.id}}</td>
                <td>{{ value.nombre }}</td>   
                <td >{{verCalificacion(value.id , 'Matematica')}}</td>
                <td >{{verCalificacion(value.id , 'Fisica')}}</td>
                <td >{{verCalificacion(value.id , 'Quimica')}}</td>
                <td> {{promedio(value.id)}}</td> 
                <td><button type="button" class="btn btn-primary" @click="editar(value)">Editar</button></td>
                <td><button type="button" class="btn btn-danger" @click="eliminar(value)">Eliminar</button></td>             
              </tr>
            </tbody>
          </table>
    </div>    
</template>

<script>
    export default{
        name:'NuevaView',
        props:[],
        emits:[],
        data() {
            return {
                Estudiantes:[],
                Calificaciones:[],
                nombreBusqueda:"",
                Estudiantesfiltro:[],
                promedioMayor:0
            }
        },
        methods: {
            getEstudiante(){
                this.axios.get("http://localhost:5000/Estudiante")
                .then((response)=>{ this.Estudiantes = response.data;})
                .catch((error)=>{console.log(error);})
            },
            getCalificaciones(){
                this.axios.get("http://localhost:5000/Calificaciones")
                .then((response)=>{ this.Calificaciones = response.data; console.log(this.Calificaciones);})
                .catch((error)=>{console.log(error);})
            },
            verCalificacion(id_estudiante,materia){  
                let notasw = 0;
                this.Calificaciones.forEach(function(calificacion) {                    
                    if (calificacion.id_estudiante === id_estudiante && calificacion.materia === materia) {
                        notasw = calificacion.nota;
                    }
                });
                return notasw;
            },
            promedio(id_estudiante){
                var n = 0;
                var sumaNota = 0;
                this.Calificaciones.forEach(function(calificacion) {                    
                    if (calificacion.id_estudiante === id_estudiante) {
                        n++;
                        sumaNota = sumaNota + calificacion.nota;
                    }
                });
                return sumaNota/n;
            },
            editar(item){
                this.$router.push('/estudiante/'+item.id +'/editar');
                //console.log(item);
            },
            RegistrarEstudiante(){
                this.$router.push('/estudianteReg');
            },
            BuscarEstudiante(){
                var buscar = this.nombreBusqueda;
                var resp=[];
                this.Estudiantes.forEach(function(estudiante) {   
                    console.log("estudiante.nombre ", estudiante.nombre, " - ", estudiante.nombre.indexOf(buscar));                 
                    if (estudiante.nombre.indexOf(buscar)==0) {
                        resp.push(estudiante);
                    }
                });
                console.log("resp ", resp);
               this.Estudiantes =resp; 
               this.Estudiantesfiltro;
            },
            BuscarEstudiantePromedioMayor(){
                var promedio = this.promedioMayor;
                var resp=[];
                console.log("this.Calificaciones",this.Calificaciones);
                this.Estudiantes.forEach(function(estudiante) {   
                    var n = 0;
                    var sumaNota = 0;
                    this.Calificaciones.forEach(function(calificacion) {                    
                        if (calificacion.id_estudiante === estudiante.id) {
                            n++;
                            sumaNota = sumaNota + calificacion.nota;
                        }
                    });
                    var pro = sumaNota/n;
                    if(pro > promedio)
                    {
                        resp.push(estudiante);
                    }
                });
                console.log("resp ", resp);
                this.Estudiantes =resp;
            },
            eliminar(item){
                console.log(item);
                this.axios.delete("http://localhost:5000/Estudiante/"+item.id)
                .then((response)=>{  console.log(response);this.getEstudiante();})
                .catch((error)=>{console.log(error);})
            }
        },
        computed:{

        },
        mounted() {
            this.getEstudiante();
            this.getCalificaciones();
        },
        components:{

        }
    }
</script>
  