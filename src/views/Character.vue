<script setup>
import { ref, onMounted, watchEffect } from "vue";
import Characters from "../../public/characters_info.json";
import Premium from "../../public/characters_premium.json";
import { useRoute } from "vue-router";
import NavBar from "../components/navbar.vue";

const characterInfo = ref(null);
const teamPremium = ref(null);
const route = useRoute();
const boolean = ref(false);
const characterId = ref(null);

const getInfo = () => {
  characterId.value = route.params.name;

  characterInfo.value = Characters.find(
    (character) => character.id === characterId.value
  );

  teamPremium.value = Premium.find(
    (character) => character.id === characterId.value
  );

  console.log(teamPremium.value);
  boolean.value = true;
};

onMounted(() => {
  getInfo();
});

watchEffect(() => {
  if (boolean.value === true) {
    if (route.params.name !== characterId.value) {
      getInfo();
    }
  }
});
</script>

<template>
  <NavBar />
  <div class="p-4 flex flex-col">
    <div
      class="block w-full p-6 bg-white border border-gray-200 rounded-lg shadow hover:bg-gray-100"
      v-if="boolean"
    >
      <img
        :src="`../../public/images/${characterInfo.id}/icon.png`"
        alt=""
        class="w-28"
      />
      <h2 class="mb-2 text-2xl font-bold tracking-tight text-gray-900">
        {{ characterInfo.name }}
      </h2>
      <div class="flex items-center">
        <svg
          aria-hidden="true"
          class="w-5 h-5 text-yellow-400"
          fill="currentColor"
          viewBox="0 0 20 20"
          xmlns="http://www.w3.org/2000/svg"
          v-for="n in characterInfo.rarity"
        >
          <title>Rarity</title>
          <path
            d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
          ></path>
        </svg>
      </div>
      <hr class="py-4" />
      <p v-if="characterInfo.gender">
        <img
          v-if="characterInfo.gender === 'Male'"
          src="../../public/icons/gender/male-gender.png"
          class="w-6"
        />
        <img
          v-else
          src="../../public/icons/gender/female-gender.png"
          class="w-6"
        />
      </p>
      <p v-if="characterInfo.title">Title: {{ characterInfo.title }}</p>
      <p>Vision: {{ characterInfo.vision }}</p>
      <p>Weapon: {{ characterInfo.weapon }}</p>
      <p>Nation: {{ characterInfo.nation }}</p>
      <p>Affiliation: {{ characterInfo.affiliation }}</p>
      <hr class="py-4" />
      <p>{{ characterInfo.description }}</p>
    </div>
    <div
      class="block w-full p-6 bg-white border border-gray-200 rounded-lg shadow hover:bg-gray-100"
    >
      <h2 class="mb-2 text-2xl font-bold tracking-tight text-gray-900">
        Premium Team
      </h2>

      <div
        class="relative overflow-x-auto shadow-md sm:rounded-lg"
        v-if="boolean"
      >
        <table class="w-full text-sm text-left text-gray-500">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50">
            <tr>
              <th
                class="px-6 py-4"
                v-for="(rol, index) in teamPremium.premium_team_roles"
                :key="index"
              >
                {{ rol }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="bg-white border-b hover:bg-gray-50">
              <td
                class="px-6 py-4"
                v-for="(teammate, index) in teamPremium.premium_team"
                :key="index"
              >
                <router-link
                  :to="'/' + teamPremium.premium_team_ids[index]"
                  :key="teamPremium.premium_team_ids[index]"
                  class="flex flex-col justify-center items-center"
                >
                  <img
                    class="h-auto max-w-full rounded-lg w-28"
                    :src="`../../public/images/${teamPremium.premium_team_ids[index]}/icon.png`"
                    alt=""
                  />
                  <p class="text-center">{{ teammate }}</p>
                </router-link>
              </td>
            </tr>
          </tbody>
        </table>
        <table class="w-full text-sm text-left text-gray-500">
          <tbody>
            <tr>
              <td class="px-6 py-4">
                <ul>
                  <li v-for="line in teamPremium.premium_team_details">
                    {{ line }}
                  </li>
                </ul>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <router-link
      class="text-gray-900 bg-gradient-to-r from-red-200 via-red-300 to-yellow-200 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-red-100 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2 max-w-sm"
      :to="'/'"
    >
      Back to Homepage
    </router-link>
  </div>
</template>

<style lang="scss" scoped></style>
