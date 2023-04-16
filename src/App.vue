<script lang="ts" setup >
import { Ref, ref } from 'vue';

interface arrayInterface {
  title: string;
  time: number;
  users: string;
}

interface arraySelectElementInterface {
  index: number;
  list: string;
}

  const isediting = ref(false)
  let hasError: Ref<string> = ref('');
  let tacheNumber: number;
  const arrayAFaire: Ref<arrayInterface[]> = ref([]);
  const arrayInProgress: Ref<arrayInterface[]> = ref([]);
  const arrayDone: Ref<arrayInterface[]> = ref([]);
  const selectedElements: Ref<arraySelectElementInterface[]> = ref([]);



  const form: Ref<arrayInterface> = ref({
    title: '',
    time: 0,
    users: ''
  })

  const formModified: Ref<arrayInterface> = ref({
    title: '',
    time: 0,
    users: ''
  })

  const modifieArray = () => {
    arrayAFaire.value[tacheNumber].title = formModified.value.title;
    arrayAFaire.value[tacheNumber].time = formModified.value.time;
    arrayAFaire.value[tacheNumber].users = formModified.value.users;
    isediting.value = false;
    formModified.value = {
      title: '',
      time: 0,
      users: ''
    };
  }

  const tacheAFaire = () => {
    if(form.value.title === '') {
      hasError.value = 'Le titre est vide';
    } else if (form.value.time === 0){
      hasError.value = 'Le temps est vide';
    } else if (form.value.users === '') {
      hasError.value = 'Utilisateur non sélectionné';
    } else if (isNaN(form.value.time)) {
      hasError.value = 'Le champ est pas un nombre';
    }
    else if (countTask(form.value.users)){
      hasError.value = "L'utilisateur à déja 3 tâches d'assigné.";
    }
    else if (countTime(form.value.users)){
      hasError.value = "Le nombre d'heure de l'utilisateur dépasse 10.";
    }
    else {
      hasError.value = '';
      arrayAFaire.value.push(form.value);
      form.value = {
        title: '',
        time: 0,
        users: ''
      };
    }
  }

  const countTime = (userSearch: string) => {
    let tasksByUser = [];
    let time: number = 0;
    
    tasksByUser = [...arrayAFaire.value.filter((x: { users: string; }) => x.users == userSearch), ...arrayInProgress.value.filter((x: { users: string; }) => x.users == userSearch), ...arrayDone.value.filter((x: { users: string; }) => x.users == userSearch)]

    for( const task of tasksByUser) {
      time += task.time;
    }

    if (time < 10) {
      return false;
    }
    else {
      return true;
    }
  }

  const countTask = (userSearch: string) => {
    let task = 0;
    task = task + arrayAFaire.value.filter((x: { users: string; }) => x.users == userSearch).length;
    task = task + arrayInProgress.value.filter((x: { users: string; }) => x.users == userSearch).length;
    task = task + arrayDone.value.filter((x: { users: string; }) => x.users == userSearch).length;

    if (task < 3) {
      return false;
    }
    else {
      return true;
    }
  }

  const inProgressAFaire = (index: number) => {
    isediting.value = false;
    arrayInProgress.value.push(arrayAFaire.value[index])
    arrayAFaire.value.splice(index, 1)
  }

  const inProgressToDone = (index: number) => {
    isediting.value = false;
    arrayDone.value.push(arrayInProgress.value[index])
    arrayInProgress.value.splice(index, 1)
  }   

  const selectElement = (index: number, list: string) => {
    isediting.value = false;
    selectedElements.value.push({
      index: index,
      list: list
    });
  }

  const deletSelectTodos = () => {
    isediting.value = false;
    for (let i = 0; i < selectedElements.value.length; i++) {
      if (selectedElements.value[i].list === "aFaire") {
        arrayAFaire.value.splice(selectedElements.value[i].index, 1);
      }
      if (selectedElements.value[i].list === "enCours") {
        arrayInProgress.value.splice(selectedElements.value[i].index, 1);
      }
      if (selectedElements.value[i].list === "termine") {
        arrayDone.value.splice(selectedElements.value[i].index, 1);
      }
    }

    selectedElements.value = [];
  }

  const supprimerAFaire = (index: number) => {
    isediting.value = false;
    arrayAFaire.value.splice(index, 1);
  }

  const supprimerInProgress = (index: number) => {
    isediting.value = false;
    arrayInProgress.value.splice(index, 1);
  }

  const archiverDone = (index: number) => {
    isediting.value = false;
    arrayDone.value.splice(index, 1);
  }

  const editElement = (index: number) => {
    isediting.value = true;
    tacheNumber = index;
  }

</script>

<template>
  <div>
    <h1 class="title-app"> ToDos App PHILIP Léo </h1>
    <div class="form-container">
      <input type="text" name="title" v-model="form.title" placeholder="Titre de la tache">
      <input type="number" name="time" v-model="form.time" placeholder="Temps de la tache">
      <select name="users" v-model="form.users">
        <option value="" disabled selected>Select an option</option>
        <option value="Léo">Léo</option>
        <option value="Yohann">Yohann</option>
        <option value="Loic">Loic</option>
        <option value="Samuel">Samuel</option>
        <option value="Macron">Macron</option>
      </select>
      <button @click="tacheAFaire">Cliquez ici</button>
    </div>
    <div v-if="hasError" class="error-message">
      {{ hasError }}
    </div>


    <div class="array-container">
      <div>
        <h1> A faire </h1>
        <div v-for="(element, index) in arrayAFaire" :key="index" class="array-todo">
          {{ element.title }}
          {{ element.time }}
          {{ element.users }}

          <button @click="inProgressAFaire(index)"> En cours </button>
          <button @click="editElement(index)"> Editer ma tache</button>
          <button @click="supprimerAFaire(index)"> Supprimer </button>
          <button @click="selectElement(index, 'aFaire')"> Selectionner</button>
        </div>
      </div>

      <div>
        <h1> En cours </h1>
          <div v-for="(element, index) in arrayInProgress" :key="index">
          {{ element.title }}
          {{ element.time }}
          {{ element.users }}

          <button @click="inProgressToDone(index)"> Termné </button>
          <button @click="supprimerInProgress(index)"> Supprimer </button>
          <button @click="selectElement(index, 'enCours')"> Selectionner</button>
        </div>
      </div>

      <div>
        <h1> Terminé </h1>
        <div v-for="(element, index) in arrayDone" :key="index">
          {{ element.title }}
          {{ element.time }}
          {{ element.users }}

          <button @click="archiverDone(index)"> Supprimer </button>
          <button @click="selectElement(index, 'termine')"> Selectionner</button>
        </div>
      </div>
    </div>

    <div v-if="isediting">
      <h1> ZONE DE MODIFICATION </h1>
      <input type="text" name="title" v-model="formModified.title" placeholder="Titre de la tache" />
      <input type="text" name="time" v-model="formModified.time" placeholder="Temps de la tache" />
      <select name="users" v-model="formModified.users">
        <option value="" disabled selected>Select an option</option>
        <option value="Léo">Léo</option>
        <option value="Yohann">Yohann</option>
        <option value="Loic">Loic</option>
        <option value="Samuel">Samuel</option>
        <option value="Macron">Macron</option>
      </select>
      <button @click="modifieArray">Cliquez ici</button>
    </div>

    <footer>
      <h3> Il y a actuellement: </h3>
      <div class="elements-footer"> 
        {{ arrayAFaire.length }} taches a faire |
        {{ arrayInProgress.length }} taches in progress |
        {{ arrayDone.length }} taches effectuées |
        {{ selectedElements.length }} éléments sélectionnées
      </div>
      <button class="delet-select-todos" @click="deletSelectTodos"> Supprimer les todos selectionnes </button>
    </footer>
  </div>

</template>

<style lang="css">
  body {
    background-color: #000000;
    color: white;
  }
  .array-container{
    display: flex;
    justify-content: space-between;
  }
  .title-app{
    text-align: center;
  }
  footer{
    text-align: center;
    position: absolute;
    bottom: 2rem;
    width: -webkit-fill-available;
  }
  .elements-footer{
    display: flex;
    justify-content: center;
  }
  .array-todo{
    margin: 0.5rem 0rem;
  }
  .form-container{
    margin: 2rem 0rem;
    display: flex;
    justify-content: center;
  }
  .delet-select-todos{
    margin-top: 1rem;
  }
</style>