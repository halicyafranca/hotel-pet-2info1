<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute } from 'vue-router';

const route = useRoute();
const API_URL='http://localhost:3000'


const tutor = ref([]);

async function carregarTutores() {
const idTutor = route.params.id;
const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`)

tutor.value = await respostaTutor.json()
}
onMounted(carregarTutores)
</script>
<template>
  <div>
<div>
   <h1>Nome do tutor: {{ tutor.nome}}</h1>
   <p>Número do Tutor: {{ tutor.telefone }}</p>

</div>
<div >
 <RouterLink :to="`/tutores/edit/${tutor.id}`"   class="btn btn-primary">Atualizar dados</RouterLink>
 <p></p>
   <RouterLink :to="{name: 'tutores'}"  class="btn btn-primary">Voltar</RouterLink>
   </div>
</div>
</template>
