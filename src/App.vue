<script setup>
import { reactive, ref, watch, onMounted } from "vue";
import { uid } from "uid";
import HeaderComponent from "./components/HeaderComponent.vue";
import Formulario from "./components/Formulario.vue";
import PacienteComponent from "./components/PacienteComponent.vue";

const pacientes = ref([]);
const paciente = reactive({
  id: null,
  mascota: "",
  propietario: "",
  email: "",
  sintomas: "",
  alta: "",
});

+
watch(
  pacientes,
  () => {
    guardarLocalStorage();
  },
  { deep: true }
);
const guardarLocalStorage = () => {
  localStorage.setItem("pacientes", JSON.stringify(pacientes.value));
};

onMounted(() => {
  const pacientesStorage = localStorage.getItem("pacientes");
  if (pacientesStorage) {
    pacientes.value = JSON.parse(pacientesStorage);
  }
});

const guardarPaciente = () => {
  if (paciente.id) {
    const { id } = paciente;
    const i = pacientes.value.findIndex((paciente) => paciente.id === id);
    pacientes.value[i] = { ...paciente };

    console.log("Editando....");
  } else {
    pacientes.value.push({ ...paciente, id: uid().toUpperCase() });
  }

  Object.assign(paciente, {
    mascota: "",
    propietario: "",
    email: "",
    sintomas: "",
    alta: "",
    id: null,
  });
};

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.filter(
    (paciente) => paciente.id === id
  )[0];
  Object.assign(paciente, pacienteEditar);
};

const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter((paciente) => paciente.id !== id);

  console.log("Elminmando paciente...", id);
};
</script>

<template>
  <div class="container mx-auto mt-20">
    <HeaderComponent />

    <div class="mt-12 md:flex">
      <Formulario
        v-model:mascota="paciente.mascota"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="paciente.id" />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll text-center">
        <h3 class="font-black text-3xl uppercase">Informacion de pacientes</h3>
        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Consulta Pacientes y
            <span class="text-indigo-600 font-bold">Administrarlos </span>
          </p>

          <PacienteComponent
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-pacientes="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente" />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
