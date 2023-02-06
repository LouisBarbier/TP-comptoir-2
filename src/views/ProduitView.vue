<template>
  <main>
    <div>
      <table>
        <caption>Les produits - {{data.page+1}}/{{max}}</caption>
        <tr>
          <th>Nom</th>
          <th>Prix</th>
          <th>Stock</th>
          <th>Commandés</th>
        </tr>
        <tr v-if="!data.listeProduits">
          <td colspan="4">Veuillez patienter, chargement des produits...</td>
        </tr>
        <tr v-for="produit in data.listeProduits" :key="produit.code">
          <td>{{ produit.nom }}</td>
          <td>{{ produit.prixUnitaire }}</td>
          <td>{{ produit.unitesEnStock }}</td>
          <td>{{ produit.unitesCommandees }}</td>
        </tr>
      </table>
    </div>
    <button @click="firstP()">first</button>
    <button @click="previousP">prev</button>
    <button @click="nextP()">next</button>
    <button @click="lastP()">last</button>
  </main>
</template>

<script setup>
import { reactive, onMounted } from "vue";
import { BACKEND, doAjaxRequest } from "../api";

let data = reactive({
  listeProduits: [],
  page:0
});

let max=0;

function chargeProduits() {
  doAjaxRequest(BACKEND + "/api/produits?sort=nom&page="+data.page+"&size=5")
      .then((json) => {
        max=json.page.totalPages;
        data.listeProduits = json._embedded.produits;
      })
      .catch((error) => alert(error.message));
}

onMounted(chargeProduits);

function firstP (){
  data.page=0;
  chargeProduits()
}
function previousP (){
  if (data.page-1>=0){
    data.page=data.page-1;
    chargeProduits()
  }
}
function nextP (){
  if (data.page+1<max){
    data.page=data.page+1;
    chargeProduits()
  }
}
function lastP (){
  data.page=max-1;
  chargeProduits()
}

</script>


<style scoped>
td,
th {
  border: 1px solid #ddd;
  padding: 8px;
}


th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #232623;
  color: rgb(255, 255, 255);
}

button{
  width: 109px;
  height: 50px;
  background-color: #232623;
  color: rgb(255, 255, 255);
}
</style>