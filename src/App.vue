<script setup>
  import { ref, reactive, onMounted, watch } from 'vue';

  //importamos un fake id
  import { uid } from 'uid';


  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import ListaAdministra from './components/ListaAdministra.vue';

  const pacientes = ref([])

   //Read data - Leyendo datos
  const paciente = reactive({
        id: null,
        nombre: '',
        propietario: '',
        email: '',
        alta: '',
        sintomas:''
    })

    const guardarPaciente = () =>{
      // console.log('guardando..')

      if(paciente.id){
        console.log('Editando..')
        const { id } = paciente;
        const indice = pacientes.value.findIndex( (pacienteEstado) => pacienteEstado.id === id )
        //como pacientes esta en un arreglo le pasamos la posicion del elemento en base al id
        pacientes.value[indice] = { ...paciente }
        
      }else{
        // console.log('Guardando...')
          pacientes.value.push({
          ...paciente,
          id: uid()

        })
      }

      //Reiniciar/limpiar Objeto paciente reactive del formulario
        paciente.nombre= ''
        paciente.propietario= ''
        paciente.email= ''
        paciente.alta= ''
        paciente.sintomas=''
        id: null
    }

    const editandoPaciente = (id) =>{
      // console.log('editando', id)
      const pacienteEditar= pacientes.value.filter(paciente => paciente.id === id)[0]
      Object.assign(paciente, pacienteEditar)

    }

    const eliminarPaciente = (id) =>{
      pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
    }


    const guardarEnLocalStorage = () =>{
      localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
    }

    onMounted(()=>{
      const pacienteStorage = localStorage.getItem('pacientes')
      if(pacienteStorage){
        pacientes.value = JSON.parse(pacienteStorage)
      }
    })

    watch(pacientes,()=>{
        guardarEnLocalStorage()
      },{
        deep: true
    })
    
</script>

<template>
    <div class="container mx-auto mt-20">
      <Header/>
      
        <div class="mt-12 md:flex">
          <Formulario
            v-model:nombre="paciente.nombre"
            v-model:propietario="paciente.propietario"
            v-model:email="paciente.email"
            v-model:alta="paciente.alta"
            v-model:sintomas="paciente.sintomas"
            @guardar-paciente="guardarPaciente"

            :id="paciente.id"
          />
          <!-- Le pasamos el id al formulario, 
          para que cuando que vayamos a llamar al componente,
           estara leyendo null o el id -->

          <div class="md:w-1/2 md:h-screen overflow-y-scroll">
            <h3 class="font-black text-3xl text-center"> Administra tus pacientes</h3>

            <div v-if="pacientes.length > 0">
              <ListaAdministra
                v-for="paciente in pacientes"
                v-bind:paciente="paciente"
                @editando-paciente="editandoPaciente"
                @eliminar-paciente="eliminarPaciente"
              />
            </div>
            <p v-else class="mt-10 text-2xl text-center">No hay pacientes</p>

          </div>
        </div>

    </div>
</template>

