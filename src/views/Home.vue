<script setup>
import {
  ref,
  onUpdated,
  watchEffect,
  reactive,
  onMounted,
  computed,
} from "vue";
import Characters from "../../public/characters_info.json";
import NavBar from "../components/navbar.vue";
</script>

<template>
  <NavBar />
  <div class="grid grid-cols-2 md:grid-cols-3 gap-4 px-4 py-8">
    <div v-for="character in Characters" :key="character.id">
      <router-link
        :to="'/' + character.id"
        class="flex flex-col justify-center items-center"
      >
        <img
          class="h-auto max-w-full rounded-lg w-28"
          :src="`../../public/images/${character.id}/icon.png`"
          alt=""
        />
        <p class="text-center">{{ character.name }}</p>
      </router-link>
    </div>
  </div>
</template>
