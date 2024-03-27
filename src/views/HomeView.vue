<script setup>
import { ref } from "vue";
import TheHeader from "../components/TheHeader.vue";
import FormComponent from "../components/FormComponent.vue";

const heberg = ref("");
const price =ref(0);

const onUpdateTotal = ({hebergement, prix}) => {
  // console.log("objet du parent:", {hebergement, prix});
  heberg.value = hebergement;
  price.value = prix;
};


</script>

<template>
  <TheHeader />
  <div class="container">
    <div class="row">
      <FormComponent @onFormChange="onUpdateTotal" />

      <div class="col-4">
        <div class="card" style="width: 18rem">
          <img v-if="!heberg" :src="'/src/assets/img/default.jpg'" class="card-img-top" alt="" />
          <img v-else :src="`/src/assets/img/${heberg}.jpg`" class="card-img-top" alt="" />
          <div class="card-body">
            <h6 class="card-title text-center">Total : {{price}} €</h6>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
