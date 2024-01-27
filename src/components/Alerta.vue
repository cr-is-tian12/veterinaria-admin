<script setup>
    import { computed } from 'vue';

    //!Comprender mas computed
    const props = defineProps({
        alerta:{
            type: Object,
            required: true
        }
    })

    const esError = computed( ()=> {
       return props.alerta.tipo === 'error'
    })


</script>

<template>
  <div
    v-bind:class="[ esError ? 'bg-red-500' : 'bg-green-500' ]"

    class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md"
  
    >
    {{ alerta.mensaje }}
  </div>
</template>