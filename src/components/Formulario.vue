<script setup>
import { reactive, computed } from "vue";
import Alerta from "./Alerta.vue";

const alerta = reactive({
  tipo: "",
  mensaje: "",
});

const emit = defineEmits([
  "update:mascota",
  "update:propietario",
  "update:email",
  "update:alta",
  "update:sintomas",
  "guardar-paciente",
]);

const props = defineProps({
  id: {
    type: [String, null],
    required: true,
  },
  mascota: {
    type: String,
    required: true,
  },

  propietario: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  alta: {
    type: String,
    required: true,
  },
  sintomas: {
    type: String,
    required: true,
  },
});

const validar = () => {
  if (Object.values(props).includes("")) {
    alerta.mensaje = "Todo los campos son obligatorios";
    alerta.tipo = "error";

    return;
  }

  emit("guardar-paciente", 123);
  alerta.mensaje = "Paciente registrado exitosamente";
  alerta.tipo = "exito";

  setTimeout(() => {
    Object.assign(alerta, {
      tipo: "",
      mensaje: "",
    });
  }, 1000);
};

const editando = computed(() => {
  return props.id;
});
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center uppercase">
      Seguimiento Pacientes
    </h2>
    <p class="text-lg mt-5 text-center mb-10">
      Agrega Pacientes y {{ propietario }}
      <span class="text-indigo-600 font-bold">Administrarlos </span>
    </p>

    <Alerta v-if="alerta.mensaje" :alerta="alerta" />

    <form
      @submit.prevent="validar"
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10">
      <div class="mb5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">
          Nombre Mascota
        </label>
        <input
          :value="mascota"
          @input="$emit('update:mascota', $event.target.value)"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          type="text"
          id="mascota"
          placeholder="Nombre de la mascota" />
      </div>
      <div class="mb5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold">
          Nombre del Propietario
        </label>
        <input
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          type="text"
          id="propietario"
          placeholder="Nombre del propietario" />
      </div>
      <div class="mb5">
        <label for="email" class="block text-gray-700 uppercase font-bold">
          EMAIL
        </label>
        <input
          :value="email"
          @input="$emit('update:email', $event.target.value)"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          type="email"
          id="email"
          placeholder="nombre@dominio.com" autocomplete="email" />
      </div>

      <div class="mb5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">
          Alta
        </label>
        <input
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          type="date"
          id="alta"
          placeholder="nombre@dominio.com" />
      </div>
      <div class="mb5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">
          Sintommas
        </label>
        <textarea
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
          type="date"
          id="sintomas"
          placeholder="Describe los intomas de la mascota"></textarea>
      </div>
      <input
        class="bg-indigo-600 w-full p-3 mt-5 text-white uppercase font-bold hove:bg-indigo-700 cursor-pointer transition-colors"
        type="submit"
        :value="[editando ? 'Guardar Cambios' : 'Registrar Paciente']" />
    </form>
  </div>
</template>
