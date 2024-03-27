<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const lastName = ref("");
const firstName = ref("");
const hebergement = ref("");
const optionKayak = ref(false);
const optionDraps = ref(false);
const ptiDej = ref("true");
const price = ref(0);

const emit = defineEmits(["onFormChange"]);

const changedForm = () => {
  price.value = 0;
  const hebergementPrices = {
    tente: 30,
    toile: 50,
    pierre: 100,
  };

  price.value += hebergementPrices[hebergement.value] || 0;

  const options = [optionKayak, optionDraps];
  const optionPrices = [30, 5];

  options.forEach((option, index) => {
    if (option.value) {
      price.value += optionPrices[index];
    }
  });

  if (ptiDej.value === "true") {
    price.value += 10;
  }

  const recapCommande = {
    nom: lastName.value,
    prenom: firstName.value,
    hebergement: hebergement.value,
    kayak: optionKayak.value,
    draps: optionDraps.value,
    ptiDej: ptiDej.value,
    prix: price.value,
  };
  // console.log("objet de enfant", recapCommande);
  emit("onFormChange", {hebergement:recapCommande.hebergement, prix:price.value});
  const recapJSON = JSON.stringify(recapCommande);
  localStorage.setItem("recapCommande", recapJSON);
};

const submittedForm = ()=>{
  //verif si champs vides + stop fct 
if (!lastName.value || !firstName.value || !hebergement.value || hebergement.value=='default') {
    alert('Veuillez remplir tous les champs obligatoires.');
    return;
  } else {
    router.push({name:'recap'});
  }
}
</script>

<template>
  <div class="col-8">
    <div class="card mb-4 shadow-sm">
      <div class="card-header">
        <h4 class="my-0 font-weight-normal">Configurer votre séjour</h4>
      </div>
      <div class="card-body">
        <div class="row mb-2">
          <div class="col">
            <input
              v-model="lastName"
              type="text"
              class="form-control"
              placeholder="Nom"
              required
            />
          </div>
          <div class="col">
            <input
              v-model="firstName"
              type="text"
              class="form-control"
              placeholder="Prénom"
              required
            />
          </div>
        </div>

        <!-- Select   -->
        <label for="type">Type d'hebergement : </label>
        <select
          v-model="hebergement"
          @change="changedForm"
          class="custom-select d-btypelock w-100"
          id="type"
          required
        >
          <option value="default" selected>Choisissez...</option>
          <option value="tente">Emplacement Tentes</option>
          <option value="toile">Camp</option>
          <option value="pierre">Pierre</option>
        </select>

        <hr class="mb-4" />
        <!-- Checkboxes button  -->
        <h4 class="my-2">Les options de séjour</h4>
        <div class="custom-control custom-checkbox">
          <input
            v-model="optionKayak"
            @change="changedForm"
            type="checkbox"
            class="custom-control-input"
            name="optionsSejour"
            value="kayak"
            id="ok-kayak"
          />
          <label class="custom-control-label" for="ok-kayak"
            >Location Kayak (+30€)</label
          >
        </div>
        <div class="custom-control custom-checkbox">
          <input
            v-model="optionDraps"
            @change="changedForm"
            type="checkbox"
            class="custom-control-input"
            name="optionsSejour"
            value="draps"
            id="ok-draps"
          />
          <label class="custom-control-label" for="ok-draps"
            >Draps (+5€)
          </label>
        </div>
        <h4 class="mt-3">Petit Déjeuner</h4>

        <div class="d-block my-3">
          <div class="custom-control custom-radio">
            <input
              v-model="ptiDej"
              @change="changedForm"
              id="ouiPetitDej"
              name="petitDej"
              type="radio"
              class="custom-control-input"
              checked
              value="true"
            />
            <label class="custom-control-label" for="ouiPetitDej"
              >Oui (+10€)</label
            >
          </div>
          <div class="custom-control custom-radio">
            <input
              v-model="ptiDej"
              @change="changedForm"
              id="nonPetitDej"
              name="petitDej"
              type="radio"
              class="custom-control-input"
              value="false"
            />
            <label class="custom-control-label" for="nonPetitDej">Non</label>
          </div>
        </div>
        <div
          v-if="!hebergement || hebergement=='default'"
          class="alert alert-warning"
          role="alert"
        >
          Erreur, merci de séléctionner un hébergement
        </div>
        <div class="mt-2">
          <button
            @click="submittedForm"
            id="submitButton"
            type="button"
            class="btn btn-lg btn-block btn-primary"
          >
            Ok
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
