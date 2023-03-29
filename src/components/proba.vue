<template>
  <div>
    <h2>{{ character.name }}</h2>

    <h4>Premium team:</h4>
    <ul>
      <li v-for="teammate in character.premium_team">{{ teammate }}</li>
    </ul>
    <p>{{ character.premium_team_details }}</p>
  </div>
</template>

<script setup>
const props = defineProps({
  character: Object,
});
</script>

<style lang="scss" scoped></style>
