<script setup>
import { reactive, ref } from 'vue';
import { useRouter } from 'vue-router';

const novoTutor =ref({
  nome:'',
  telefone:''
})
const erros = reactive({nome:'', telefone:''})

function validar() {
  erros.nome = novoTutor.value.nome.trim() ? '' : 'Informe o nome do tutor.'
  erros.telefone = novoTutor.value.telefone ? '' : 'Informe o número.'


  return !erros.nome && !erros.telefone
}
const router = useRouter()
const API_URL='http://localhost:3000'
async function salvarTutor() {
  if (!validar()) return
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

 <form @submit.prevent="salvarTutor" novalidate>

      <div class="col-md-6">
      <label for="nome" class="form-label"> Nome do tutor</label>
      <input type="text" id="nome" v-model="novoTutor.nome" class="form-control"     :class="{ 'is-invalid': erros.nome }">
      <div class="invalid-feedback">{{ erros.nome }}</div>
      </div>

     <div class="col-md-6">
      <label for="nome" class="form-label"> Número do tutor</label>
      <input type="text" id="nome" v-model="novoTutor.telefone" class="form-control"     :class="{ 'is-invalid': erros.telefone }">
      <div class="invalid-feedback">{{ erros.telefone }}</div>
      </div>
  <button type="submit" class="btn btn-primary  mt-2"> salvar tutor</button>
    </form>
    </div>
</template>
