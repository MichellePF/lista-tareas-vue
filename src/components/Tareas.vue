<template>
    <v-container>
        <v-row row wrap class="ma-5">

            <v-col md="6">
                <v-card class="mb-3" v-for="(item, index) in listaTareas" :key="index">
                    <v-card-text>
                        <v-chip class="mb-3" color="pink" label text-color="white">
                            <v-icon left>mdi-label</v-icon>
                                {{item.titulo}}
                        </v-chip>
                        <p>{{item.descripcion}}</p>
                        <v-btn color="warning" class="mr-2" @click="editar(index)">Editar</v-btn>
                        <v-btn color="error" @click="eliminarTarea(item.id)">Eliminar</v-btn>
                    </v-card-text>
                </v-card>

            </v-col>

            <v-col md="6" v-if="formAgregar">
                <v-card class="mb-3 pa-3">
                    <v-form @submit.prevent="agregarTarea">
                        <v-text-field label="Titulo de tarea" v-model="titulo"></v-text-field>
                        <v-textarea label="Descripcion de tarea" v-model="descripcion"></v-textarea>
                        <v-btn block color="success" type="submit">Agregar Tarea</v-btn>
                    </v-form>
                </v-card>
            </v-col>
            <v-col md="6" v-if="!formAgregar">
                <v-card class="mb-3 pa-3">
                    <v-form @submit.prevent="editarTarea">
                        <v-text-field label="Titulo de tarea" v-model="titulo"></v-text-field>
                        <v-textarea label="Descripcion de tarea" v-model="descripcion"></v-textarea>
                        <v-btn block color="warning" type="submit">Editar Tarea</v-btn>
                    </v-form>
                </v-card>
            </v-col>

        </v-row>
        <v-snackbar v-model="snackbar" >
            {{ mensaje }}
            <v-btn class="ml-5" color="pink"  @click="snackbar = false">Close</v-btn>
        </v-snackbar>
    </v-container>
</template>

<script>
export default {
    data(){
        return {
            listaTareas: [
                {id: 1, titulo: 'Titulo Tarea #1', descripcion: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Quaerat ut similique quibusdam porro eligendi dicta doloribus, vitae consectetur quos veniam. Harum suscipit odit atque perspiciatis asperiores voluptate quaerat quisquam iusto?'},
                {id: 2, titulo: 'Titulo Tarea #2', descripcion: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Quaerat ut similique quibusdam porro eligendi dicta doloribus, vitae consectetur quos veniam. Harum suscipit odit atque perspiciatis asperiores voluptate quaerat quisquam iusto?'}
            ],
            titulo: '',
            descripcion: '',
            snackbar: false,
            mensaje: '',
            formAgregar: true,
            indexTarea: ''
        }
    },
    methods: {
        agregarTarea(){
            if(this.titulo === '' || this.descripcion === ''){
                this.snackbar = true
                this.mensaje = 'Llena todos los campos!'
            }else{
                this.listaTareas.push({
                    id: Date.now(),
                    titulo: this.titulo,
                    descripcion: this.descripcion
                })
                this.titulo = ''
                this.descripcion = ''
                this.snackbar = true
                this.mensaje = 'La Tarea ha sido agregada!'
            }
        },
        eliminarTarea(id){
            this.listaTareas = this.listaTareas.filter(e => e.id != id)
        },
        editar(index){
            this.formAgregar = false
            this.titulo = this.listaTareas[index].titulo
            this.descripcion = this.listaTareas[index].descripcion
            this.indexTarea = index
        },
        editarTarea(){
            this.listaTareas[this.indexTarea].titulo = this.titulo
            this.listaTareas[this.indexTarea].descripcion = this.descripcion
            this.formAgregar = true
            this.titulo = ''
            this.descripcion = ''
            this.snackbar = true
            this.mensaje = 'Ha sido editada la tarea!'
        }
    }
}
</script>