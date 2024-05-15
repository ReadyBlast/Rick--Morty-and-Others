<script setup>
import { onBeforeMount, reactive, ref, watch } from 'vue';
import CharacterCard from './components/CharacterCard.vue';
import BaseButton from './components/BaseButton.vue';

const defaultURL = 'https://rickandmortyapi.com/api/character?';

const characters = ref({});

const pageCounter = ref(1);
const maxPagesCounter = ref(1);

const previosPageURL = ref();
const nextPageURL = ref();

const filters = reactive({
  status: '',
  name: '',
});

const fetchDataHandler = async (url) => {
  await fetch(url)
    .then((res) => res.json())
    .then((data) => {
      characters.value = data;
      nextPageURL.value = data.info.next;
      previosPageURL.value = data.info.prev;
      maxPagesCounter.value = data.info.pages;
    })
    .catch((err) => console.error(err));
};

onBeforeMount(() => fetchDataHandler(defaultURL));

const onEnterSelectPage = (event) => {
  pageCounter.value = Number(event.target.value);
};

const onClickGoToNextPage = (url) => {
  pageCounter.value++;
  fetchDataHandler(url);
};

const onClickGoToPreviousPage = (url) => {
  pageCounter.value--;
  fetchDataHandler(url);
};

const onClickFetchSelectedData = () => {
  const url = defaultURL + new URLSearchParams(filters).toString();
  pageCounter.value = 1;
  fetchDataHandler(url);
};

const changeSelectedParams = (event) => {
  if (event.target.classList.value === 'parameters__filter--selector') {
    filters.status = event.target.value;
  }

  if (event.target.classList.value === 'parameters__search-field') {
    filters.name = event.target.value;
  }
};

watch(pageCounter, () => {
  if (pageCounter.value > maxPagesCounter.value) {
    alert('Страница выбрана неправильно');
  } else {
    const url =
      defaultURL + new URLSearchParams(Object.assign({ page: pageCounter.value}, filters)).toString();
      console.log(url)
    fetchDataHandler(url);
  }
});
</script>

<template>
  <div class="container">
    <div class="parameters">
      <select
        :onChange="changeSelectedParams"
        class="parameters__filter--selector"
      >
        <option value="">All</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <input
        class="parameters__search-field"
        type="text"
        placeholder="Search"
        :onChange="changeSelectedParams"
      />
      <BaseButton
        class="parameters__accept-button"
        @click="onClickFetchSelectedData"
      >
        Применить
      </BaseButton>
    </div>

    <div class="character-list">
      <CharacterCard
        v-for="item in characters.results"
        :key="item.id"
        :name="item.name"
        :status="item.status"
        :species="item.species"
        :image="item.image"
        :location="item.location.name"
        :episodeURL="item.episode[0]"
      />
    </div>

    <div class="pagination">
      <BaseButton
        v-if="previosPageURL"
        class="pagination__button-prev"
        @click="onClickGoToPreviousPage(previosPageURL)"
      >
        <
      </BaseButton>
      <BaseButton v-else class="pagination__button-prev" disabled><</BaseButton>
      <input
        type="text"
        class="pagination__select-page-input"
        v-model="pageCounter"
        :placeholder="pageCounter"
        @click.enter="onEnterSelectPage"
      />
      <p class="pagination__total-pages">/ {{ maxPagesCounter }}</p>
      <BaseButton
        v-if="nextPageURL"
        class="pagination__button-next"
        @click="onClickGoToNextPage(nextPageURL)"
      >
        >
      </BaseButton>
      <BaseButton v-else class="pagination__button-next" disabled>></BaseButton>
    </div>
  </div>
</template>

<style scoped>

input {
  border-radius: 5px;
  border: 1px solid #cccccc;
  outline: none;

  width: 30px;
  padding: 0 5px;
  margin: 0 15px;
}

input::placeholder {
  color: rgb(212, 212, 212);
}

.container {
  margin: auto;

  border-radius: 5px;
  padding: 10px;
  color: white;
}

.parameters {
  display: flex;
  margin: 0 5px 15px;
}

.parameters__filter--selector {
  color: white;
  background-color: rgb(60, 62, 68);
  border-radius: 5px;
  padding: 0 5px;
}

.parameters__search-field {
  width: 100%;
}

.character-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

  padding: 10px 5px;
  margin-bottom: 15px;
  /* display: grid;
  grid-template-columns: 1fr 1fr; */
  gap: 15px;
}

.pagination {
  display: flex;
}

.pagination__total-pages {
  margin-right: 15px;
}
</style>
