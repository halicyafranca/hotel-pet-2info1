<script setup >
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute, useRouter } from 'vue-router';

const router = useRouter()
const route = useRoute();
const API_URL='http://localhost:3000'
//exibir informações do pet e tutor
const pet = ref({});
const tutor = ref({});


async function carregarPet() {

const idPet = route.params.id;

console.log('Id do pet:', idPet);

const respostaPet = await fetch(`${API_URL}/pets/${idPet}`);

pet.value =await respostaPet.json();

const respostaTutor = await fetch(`${API_URL}/tutores/${pet.value.tutorId}`);

tutor.value = await respostaTutor.json()
}
async function excluirPet(id){
    if (!confirm('Tem certeza que deseja excluir este pet?')) return;
 await fetch(`${API_URL}/pets/${id}`,{
  method:'DELETE'
});
router.push('/pets');
}
onMounted(carregarPet);
</script>
<template>
  <h1>Nome do pet: {{ pet.nome }}</h1>
  <p>Espécie: {{ pet.especie }}</p>
   <p>Nome do Tutor: {{ tutor?.nome }}</p>
<div>

 <RouterLink :to="`/pets/edit/${pet.id}`"  class="btn btn-primary">Atualizar dados</RouterLink>

<button class="btn btn-primary" @click="excluirPet(pet.id)">
Excluir Pet
</button>

   <RouterLink :to="{name: 'pets'}"  class="btn btn-primary">Voltar</RouterLink>

  </div>
</template>
