<script setup>
import { onMounted, ref } from 'vue';
const episodeName = ref('');
const props = defineProps({
  name: String,
  status: String,
  species: String,
  image: String,
  location: String,
  episodeURL: String,
});

onMounted(async () => {
  try {
    fetch(props.episodeURL)
      .then((res) => res.json())
      .then((data) => (episodeName.value = data.name));
  } catch (error) {
    console.log(error);
  }
});
</script>

<template>
  <div class="character-card">
    <img
      :src="image"
      :alt="name"
      class="character-card__avatar"
    />
    <div class="character-card__container-bio">
      <h2>{{ name }}</h2>
      <h4>{{ status }} - {{ species }}</h4>
      <p>Last known location:</p>
      <b>{{ location }}</b>
      <p>First seen in:</p>
      <b>{{ episodeName }}</b>
    </div>
  </div>
</template>

<style scoped>
.character-card {
  display: flex;
  align-items: center;

  padding: 10px;
  border-radius: 12px;
  background-color: rgb(60, 62, 68);

  max-width: 500px;
  width: 100%;
}

.character-card__avatar {
  margin-right: 15px;
  height: 150px;
}

.character-card__container-bio {
  overflow-wrap: anywhere;
}
</style>
