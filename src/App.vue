<script lang="ts" setup >
import { ref } from 'vue';

  const arrayAFaire: any = ref([]);
  const arrayInProgress: any = ref([]);
  const arrayDone: any = ref([]);

  const form: any = ref({
    title: '',
    time: 0,
    users: ''
  })

  const tacheAFaire = () => {
    arrayAFaire.value.push(form.value);
    form.value = {
      title: '',
      time: 0,
      users: ''
    };
  }

  const inProgressAFaire = (index: number) => {
    arrayInProgress.value.push(arrayAFaire.slice(index, 1)[0]);
  }

  const supprimerAFaire = (index: number) => {
    arrayAFaire.valuesplice(index, 1);
  }
</script>

<template>
  <div>
    <h1> COUCOU JAI UN GROS ZIZI </h1>
    <input type="text" name="title" v-model="form.title" placeholder="Titre de la tache">
    <input type="text" name="time" v-model="form.time" placeholder="Temps de la tache">
    <select name="users" v-model="form.users">
      <option value="" disabled selected>Select an option</option>
      <option value="option1">Yohanniboule</option>
      <option value="option2">Loicccahuette</option>
      <option value="option3">Pierrot</option>
    </select>
    <button @click="tacheAFaire">Cliquez ici</button>

    <div v-if="arrayAFaire.length > 0">
      <h1> A faire </h1>
      <div v-for="(element, index) in arrayAFaire" :key="index">
        {{ element.title }}
        {{ element.time }}
        {{ element.users }}

        <button @click="inProgressAFaire(index)"> En cours </button>
        <button @click="editerAFaire(index)"> Editer</button>
        <button @click="supprimerAFaire(index)"> Supprimer </button>
      </div>
    </div>

    <!-- <div v-if="arrayInProgress.length > 0">
      <h1> En cours </h1>
      <div v-for="element in arrayAFaire">
        {{ element.title }}
        {{ element.time }}
        {{ element.users }}
      </div>
    </div>

    <div v-if="arrayDone.length > 0">
      <h1> Terminé </h1>
      <div v-for="element in arrayAFaire">
        {{ element.title }}
        {{ element.time }}
        {{ element.users }}
      </div>
    </div> -->
  </div>
</template>