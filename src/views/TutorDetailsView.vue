<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute } from 'vue-router';

const route = useRoute();
const API_URL='http://localhost:3000'


const tutor = ref({});
const pets = ref([])
async function carregarTutores() {
const idTutor = route.params.id;
const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`)

tutor.value = await respostaTutor.json()
}
async function carregarPets(){
const idTutor = route.params.id;
const respostaPets= await fetch(`${API_URL}/pets?tutorId=${idTutor}`)
pets.value = respostaPets.json()
}
onMounted(() =>{
  carregarTutores()
  carregarPets()
} )
</script>
<template>
  <header>
    <h1 class="fw-bold mb-0" >Detalhes do tutor</h1>
    <p class="text-secondary">  Visualize os dados do tutor e cadastre pets vinculados
    </p>
  </header>
  <div class="card mb-4">
  <div class="card-header bg-light fs-4 fw-semibold"> Informações do tutor </div>
  <div class="card-body">
  <div class="card mb-3" >
<div class="card-header bg-light fw-semibold">Dados do Tutor</div>
      <div class="card-body">
        <div class="row g-3">
          <div class="col-md-4">
            <div class="fw-bold">ID</div>
            <div>{{ tutor.id }}</div>
          </div>
          <div class="col-md-4">
            <div class="fw-bold">Nome Completo</div>
            <p>{{ tutor.nome }}</p>
          </div>
          <div class="col-md-4">
            <div class="fw-bold">Telefone</div>
            <p>{{ tutor.telefone }}</p>
          </div>
   </div>
  </div>
</div>
 <div class="d-flex gap-2">

 <p></p>
   <RouterLink :to="{name: 'tutores'}"  class="btn btn-secondary">Voltar</RouterLink>
   </div>
</div>
</div>
 <div class="card mb-4">
  <div class="card-header bg-light fs-4 fw-semibold"> Pets do tutor </div>
    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Especie</th>
          <th>Ações</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="pet in pets"
          :key="pet.id"
        >
          <td>{{ pet.id }}</td>
          <td>{{ pet.nome }}</td>
          <td>{{ pet.especie }}</td>
           <td>
             <RouterLink :to="`/pets/${pet.id}`" class="btn btn-secondary">Visualizar</RouterLink>
             <button class="btn btn-danger" @click="excluirPet(pet.id)"> Excluir
             </button>
           </td>

        </tr>
      </tbody>
    </table>

  </div>
</template>
