<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const novoTutor =ref({
  nome:'',
  telefone:''
})
const router = useRouter()
const API_URL='http://localhost:3000'
async function salvarTutor() {
 await fetch(`${API_URL}/tutores`,{
  method:'POST',
  header:{
    'Content-type':'application/json',
  },
  body:JSON.stringify(novoTutor.value),
});
router.push('/tutores')
}


</script>
<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro dos tutore</h1>
      <p class="text-body-secondary mb-0">Cadastro dos tutores no sistema.</p>
    </header>

 <form @submit.prevent="salvarTutor">

      <div class="col-md-6">
      <label for="nome" class="form-label"> Nome do tutor</label>
      <input type="text" id="nome" v-model="novoTutor.nome" class="form-control" required>
      </div>

     <div class="col-md-6">
      <label for="nome" class="form-label"> Número do tutor</label>
      <input type="text" id="nome" v-model="novoTutor.telefone" class="form-control" required>
      </div>
  <button type="submit" class="btn btn-secondary"> salvar tutor</button>
    </form>
    </div>
</template>
