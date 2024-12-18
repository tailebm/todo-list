<template>
  <div class="header">
    <img :src="logo" alt="Logo" class="logo" />
    <h1>Ma Todo List</h1>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import logoImg from "@/assets/to-do-list.png"; // Import du logo

export default defineComponent({
  name: "AppHeader",
  data() {
    return {
      logo: logoImg,
    };
  },
});
</script>

<style scoped>
.logo {
  width: 100px;
  height: auto;
  margin-right: 10px;
}

.header {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}

h1 {
  font-family: "Arial", sans-serif; /* Police moderne */
  font-size: 2.5rem; /* Taille plus grande */
  text-align: center; /* Centrage horizontal */
  color: #4caf50; /* Couleur verte élégante */
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2); /* Ombre douce */
  margin-bottom: 20px; /* Espacement avec les autres éléments */
  padding: 10px;
  background: linear-gradient(to right, #e0f7fa, #b2dfdb); /* Dégradé de fond */
  border-radius: 8px; /* Coins arrondis */
  display: inline-block; /* Ajustement du fond autour du texte */
}
</style>
