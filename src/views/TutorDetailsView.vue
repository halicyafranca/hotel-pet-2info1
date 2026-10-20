<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute, useRouter } from 'vue-router';

const route = useRoute();
const API_URL='http://localhost:3000';
const router = useRouter()


const tutor = ref({});
const pets = ref({});

const novoPet =ref({
  nome:'',
  especie:'',
  tutorId: route.params.id
})


async function carregarDados() {
const idTutor = route.params.id;
const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`)

tutor.value = await respostaTutor.json()

const respostaPets= await fetch(`${API_URL}/pets`)
pets.value = await respostaPets.json()
pets.value = pets.value.filter(p => p.tutorId === String(idTutor))
console.log(pets.value)

}
async function salvarPet() {
 await fetch(`${API_URL}/pets`,{
  method:'POST',
  header:{
    'Content-type':'application/json',
  },
  body:JSON.stringify(novoPet.value),

});
router.push('/pets');
}

async function excluirPet(id) {
  const confirmarExclusao= window.confirm('Deseja realmente excluir o pet?');

  if(confirmarExclusao){
    //logica
    await fetch(`${API_URL}/pets/${id}`,{
      method:'DELETE',
    });
    //RECARREGA

  await carregarDados()

}
}
onMounted(carregarDados)
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
<div class="container-fluid d-flex gap-2">
 <div class="flex-grow-1 card mb-2">
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
   <div class=" flex-grow-1 card mb-2">
     <div class="card-header bg-light fs-4 fw-semibold"> Cadastre um novo Pet </div>
 <form @submit.prevent="salvarPet">

      <div class="col-md-6 ms-2">
      <label for="nome" class="form-label"> nome do pet</label>
      <input type="text" id="nome" v-model="novoPet.nome" class="form-control" required >
      </div>

       <div class="col-md-6 ms-2">
      <label for="especie" class="form-label"> especie </label>
      <select  id="especie"
      v-model="novoPet.especie"
      class="form-select"
      required>
   <option value="" disabled> selecionar a especie</option>
   <option value="Gato">Gato</option>
   <option value="Cachorro">Cachorro</option>
      </select>
      </div>
  <button type="submit" class="btn btn-secondary"> salvar</button>
    </form>
   </div>
   </div>
</template>
