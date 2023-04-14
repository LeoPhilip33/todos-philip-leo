<script lang="ts" setup >
import { ref } from 'vue';

  const isediting = ref(false)
  const arrayAFaire: any = ref([]);
  const arrayInProgress: any = ref([]);
  const arrayDone: any = ref([]);

  const form: any = ref({
    title: 'RIEN',
    time: 0,
    users: 'RIEN'
  })

  const tacheAFaire = () => {
    arrayAFaire.value.push(form.value);
    form.value = {
      title: 'RIEN',
      time: 0,
      users: 'RIEN'
    };
  }

  const inProgressAFaire = (index: number) => {
    arrayInProgress.value.push(arrayAFaire.value[index])
    arrayAFaire.value.splice(index, 1)
  }

  const inProgressToDone = (index: number) => {
    arrayDone.value.push(arrayInProgress.value[index])
    arrayInProgress.value.splice(index, 1)
  }   

  const supprimerAFaire = (index: number) => {
    arrayAFaire.value.splice(index, 1);
  }

  const supprimerInProgress = (index: number) => {
    arrayInProgress.value.splice(index, 1);
  }

  const archiverDone = (index: number) => {
    arrayDone.value.splice(index, 1);
  }

  const editElement = (index: number, element: string) => {
    isediting.value = true;

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
        <button @click="editElement(index, 'tamer')"> Editer</button>
        <button @click="supprimerAFaire(index)"> Supprimer </button>
      </div>
    </div>

     <div v-if="arrayInProgress.length > 0">
      <h1> En cours </h1>
        <div v-for="(element, index) in arrayInProgress" :key="index">
        {{ element.title }}
        {{ element.time }}
        {{ element.users }}

        <button @click="inProgressToDone(index)"> Done </button>
        <!-- <button @click="editerAFaire(index)"> Editer</button> -->
        <button @click="supprimerInProgress(index)"> Supprimer </button>
      </div>
    </div>

    <div v-if="arrayDone.length > 0">
      <h1> Terminé </h1>
      <div v-for="(element, index) in arrayDone" :key="index">
        {{ element.title }}
        {{ element.time }}
        {{ element.users }}

        <button @click="archiverDone(index)"> Supprimer </button>
      </div>
    </div>

    <div>
      Il y a actuellement: <br>
      {{ arrayAFaire.length }} taches <br>
      {{ arrayInProgress.length }} taches in progress <br>
      {{ arrayDone.length }} taches effectuées
    </div>
  </div>

  <div v-if="isediting">
    <h1> ZONE DE MODIFICATION </h1>

  </div>
</template>