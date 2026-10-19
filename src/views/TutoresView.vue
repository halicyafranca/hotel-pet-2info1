<script setup>
import { onMounted, ref } from 'vue';

const tutores = ref([]);
const pets = ref([]);
const API_URL='http://localhost:3000'
async function carregarTutores(){
  const respostaTutor = await fetch(`${API_URL}/tutores`)
  tutores.value = await respostaTutor.json()
  return tutores
}
async function carregarPets(){
  const respostaPets = await fetch(`${API_URL}/pets`)
 pets.value = await respostaPets.json();
}

 function petsTutor(tutorId){
  return pets.value.filter(pet => Number(pet.tutorId) === Number(tutorId))
}

async function excluirTutor(id){
const confirmarExclusao= window.confirm('Deseja realmente excluir o tutor?');
if(confirmarExclusao){
await fetch(`${API_URL}/tutores/${id}`, {
  method:'DELETE',
})
 await carregarTutores()
}
}
onMounted(()=> {
  carregarTutores()
  carregarPets()
})
</script>
<template>
  <div >
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem dos Tutores</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Tutores cadastrados no sistema.
      </p>
    </header>
 <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addTutor' }"
    >
      Adicionar Tutor
    </RouterLink>

  <table  class="table table-striped table-hover">
  <thead>
  <tr>
   <th>Id</th>
   <th>Nome</th>
   <th>Telefone</th>
   <th>Pets(Quantidade)</th>
   <th>Nome dos Pets</th>
   <th>Ações</th>
   </tr>
   </thead>
      <tbody>
        <tr
          v-for="tutor in tutores"
          :key="tutor.id"
        >
          <td>{{ tutor.id }}</td>
          <td>{{ tutor.nome }}</td>
          <td>{{ tutor.telefone }}</td>
           <td>{{ petsTutor(tutor.id).length}}</td>
           <td>
             {{ petsTutor(tutor.id).map(p => p.nome).join(', ')|| '-' }}
           </td>
           <td>
             <button class="btn btn-danger" @click="excluirTutor(tutor.id)"> Excluir
             </button>
             <RouterLink :to="`/tutores/${tutor.id}`" class="btn btn-primary">Visualizar</RouterLink>
           </td>

        </tr>
      </tbody>
  </table>
  </div>
</template>

