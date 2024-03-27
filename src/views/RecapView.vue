<script setup>
import { ref } from "vue";

const recapJSON = localStorage.getItem("recapCommande");
const recapCommande = ref({});

if (recapJSON) {
  recapCommande.value = JSON.parse(recapJSON);
}

console.log(recapCommande);
</script>
<template>
  <div class="container d-flex justify-content-center">
    <div class="card" style="width: 18rem">
      <img
        :src="`/src/assets/img/${recapCommande.hebergement}.jpg`"
        class="card-img-top"
        alt="default"
      />
      <div class="card-body">
        <h5 class="card-title">Votre Récap</h5>
        <p class="card-text">
          Nous vous souhaitons un bon séjour
          <strong>{{ recapCommande.nom }}</strong> {{ recapCommande.prenom }}
        </p>
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item">
          Hébergement: <strong>{{ recapCommande.hebergement }}</strong>
        </li>
        <li class="list-group-item">
          Vos options :<span v-if="recapCommande.kayak">Kayak ⛴</span>
          <span v-if="recapCommande.kayak && recapCommande.draps">&</span>
          <span v-if="recapCommande.draps">Draps</span>
        </li>
        <li class="list-group-item">
          <strong
            >Options déjeuner :
            <span v-if="recapCommande.ptiDej == 'true'">Oui</span>
            <span v-else>Non</span>
          </strong>
        </li>
        <li class="list-group-item">
          Reste à payer : {{ recapCommande.prix }} €
        </li>
      </ul>
      <div class="card-body">
        <button
          id="submitButton"
          type="button"
          class="btn btn-lg btn-block btn-primary"
        >
          Confirmer
        </button>
      </div>
    </div>
  </div>
</template>
<style scoped></style>
