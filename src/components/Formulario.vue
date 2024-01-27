<script setup>
    import { reactive, computed } from 'vue';
    import Alerta from './Alerta.vue';

    const alertaMensaje = reactive ({
        tipo: '',
        mensaje: ''
    })
    
    const validarBtn = () =>{
        if(Object.values(props).includes('')){
            alertaMensaje.mensaje = 'Todos los campos son obligatorios';
            alertaMensaje.tipo = 'error';
            
                setTimeout(()=>{
                    alertaMensaje.mensaje = ''
                    alertaMensaje.tipo = ''
                }, 3000);
            return
        }

        emit('guardar-paciente');
        alertaMensaje.mensaje = 'Agregado Correctamente';
        alertaMensaje.tipo = 'exito';
            setTimeout(() => {
                Object.assign(alertaMensaje, {                
                    mensaje: '',
                    tipo: ''
                })

            }, 3000);
    }

    //declaramos los emits para que se escuchen
    const emit = defineEmits(['update:nombre','update:propietario', 'update:email', 'update:alta','update:sintomas', 'guardar-paciente'])


    /* declaramos las propiedades para que exista 
    y se puedan comunicar entre ellas */
    const props = defineProps({
        id:{
            type: [String, null],
            required: true
        },
        nombre:{
            type: String,
            required: true
        },
        propietario:{
            type: String,
            required: true
        },
        email:{
            type: String,
            required: true
        },
        alta:{
            type: String,
            required: true
        },
        sintomas:{
            type: String,
            required: true
        }
    })

    const editando = computed(()=>{
        return props.id
    }) 
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento de pacientes</h2>
        
        <p class="text-lg mt-5 text-center">
            Añade Pacientes y 
            <span class="text-indigo-600">Admínistralos</span>
        </p>

        <Alerta
            v-if="alertaMensaje.mensaje"
            v-bind:alerta="alertaMensaje"
        />
        
        <form 
        class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
        v-on:submit.prevent="validarBtn"
        >
        
            <div class="mb-5">
                <label 
                for="mascota"
                class="block text-gray-700 uppercase font-bold"
                >
                    Nombre mascota
                </label>
                
                <input 
                id="mascota" 
                type="text"
                placeholder="Nombre de mascota"
                class="border-2 w-full mt-2 p-2 placeholder-gray-400 rounded-md"
                :value="nombre"
                @input="$emit('update:nombre', $event.target.value)"
                />
            </div>

            <div class="mb-5">
                <label 
                for="propietario"
                class="block text-gray-700 uppercase font-bold"
                >
                    Nombre propietario
                </label>
                
                <input 
                id="propietario" 
                type="text"
                placeholder="Nombre de propietario"
                class="border-2 w-full mt-2 p-2 placeholder-gray-400 rounded-md"
                :value="propietario"
                @input="$emit('update:propietario', $event.target.value)"
                />
            </div>

            <div class="mb-5">
                <label 
                for="email"
                class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                
                <input 
                id="email" 
                type="email"
                placeholder="E-mail"
                class="border-2 w-full mt-2 p-2 placeholder-gray-400 rounded-md"
                :value="email"
                @input="$emit('update:email', $event.target.value)"
                />
            </div>

            <div class="mb-5">
                <label 
                for="alta"
                class="block text-gray-700 uppercase font-bold"
                >
                    Alta
                </label>
                
                <input 
                id="email" 
                type="date"
                class="border-2 w-full mt-2 p-2 placeholder-gray-400 rounded-md"
                :value="alta"
                @input="$emit('update:alta', $event.target.value)"
                />
            </div>

            <div class="mb-5">
                <label 
                for="sintomas"
                class="block text-gray-700 uppercase font-bold"
                >
                    Sintomas
                </label>
                
                <textarea 
                id="sintomas" 
                placeholder="Descripción de sintomas"
                class="border-2 w-full mt-2 p-2 placeholder-gray-400 rounded-md h-40"
                :value="sintomas"
                @input="$emit('update:sintomas', $event.target.value)"
                ></textarea>
            </div>

            <input 
            type="submit"
            class="bg-indigo-600 font-bold w-full p-3 uppercase text-white cursor-pointer hover:bg-indigo-700"
            :value="[editando ? 'Guardar cambios' : 'Registrar Pacientes']"
            />

        </form>
    </div>
</template>