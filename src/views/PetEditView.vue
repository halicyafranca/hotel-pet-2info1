<script setup >
import { onMounted, ref } from 'vue';
import {useRoute, useRouter } from 'vue-router';

const router = useRouter()
const route = useRoute();
const API_URL='http://localhost:3000'
//exibir informações do pet e tutor
const pet = ref({});
const tutor = ref({});

const novoNome = ref("");
const novaEspecie = ref("");
const nomeTutor = ref("");

async function salvarAtt(id) {
 await fetch(`${API_URL}/pets/${id}`,{
  method:'PATCH',
  header:{
    'Content-type':'application/json',
  },
  body:JSON.stringify({
    nome: novoNome.value,
    especie:novaEspecie.value,
    tutor:nomeTutor.value
  }),

});
router.push('/pets');
}
async function carregarPet() {

const idPet = route.params.id;

console.log('Id do pet:', idPet);

const respostaPet = await fetch(`${API_URL}/pets/${idPet}`);

pet.value =await respostaPet.json();

const respostaTutor = await fetch(`${API_URL}/tutores/${pet.value.tutorId}`);

tutor.value = await respostaTutor.json()
}

onMounted(carregarPet);
</script>
<template>
 <form @submit.prevent="salvarAtt(pet.id)"  >
 <div class="col-md-6">
<label for="nome" class="form-label">Atualize o Nome</label>
<input type="text" :placeholder="`${pet.nome}`" v-model="novoNome" class="form-control" >
</div>
<div class="col-md-6">
<label for="especie" class="form-label">Atualize a Especie</label>
<input type="text" :placeholder="`${pet.especie}`" v-model="novaEspecie" class="form-control" >
</div>
<div class="col-md-6">
<label for="nomeTutor" class="form-label">Atualize o Nome do tutor</label>
<input type="text" :placeholder="`${tutor.nome}`" v-model="nomeTutor" class="form-control" >
</div>

<button type="submit"  class="btn btn-primary">Salvar informações</button>
 </form>
</template>
