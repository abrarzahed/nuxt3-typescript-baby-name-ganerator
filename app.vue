<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" button bellow</p>
    <div class="options-container">
      <Option
        v-for="option in optionsArray"
        :key="option.title"
        :option="option"
        :options="options"
      />
      <button class="primary-btn" @click="computeSelectedNames">
        Find Names
      </button>

      <div
        class="cards-container"
        :class="selectedNames.length > 0 && 'available'"
      >
        <CardName
          v-for="(name, i) in selectedNames"
          :key="i"
          :name="name"
          @remove="removeName(i)"
          :index="i"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Gender, Popularity, Length, names } from "@/data";
interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length;
}
const options = reactive<OptionsState>({
  gender: Gender.GIRL,
  length: Length.SHORT,
  popularity: Popularity.TRENDY,
});

const selectedNames = ref<string[]>([]);

const removeName = function (index) {
  const filteredNames = [...selectedNames.value];
  filteredNames.splice(index, 1);
  selectedNames.value = filteredNames;
};

const computeSelectedNames = () => {
  const filteredNames = names
    .filter((name) => name.gender === options.gender)
    .filter((name) => name.popularity === options.popularity)
    .filter((name) => {
      if (options.length === Length.ALL) return true;
      else return name.length === options.length;
    });
  selectedNames.value = filteredNames.map((name) => name.name);
};
const optionsArray = [
  {
    title: "1) Choose a gender",
    category: "gender",
    buttons: [Gender.GIRL, Gender.BOY, Gender.UNISEX],
  },
  {
    title: "2) Choose the name's popularity",
    category: "popularity",
    buttons: [Popularity.TRENDY, Popularity.UNIQUE],
  },
  {
    title: "3) Choose name's length",
    category: "length",
    buttons: [Length.SHORT, Length.ALL, Length.LONG],
  },
];
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}
.container {
  color: #555;
  max-width: 40rem;
  margin: 2rem auto;
  padding: 1.5rem;
  display: grid;
  gap: 2rem;
  border-radius: 4px;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.15);
  line-height: 1.3;
}
.options-container {
  display: grid;
  gap: 2rem;
}
.option-buttons {
  display: flex;
  gap: 1rem;
  margin-top: 0.5rem;
  flex-wrap: wrap;
}

button {
  padding: 0.8rem 1.5rem;
  border-radius: 4px;
  border: 1px solid #0747dd;
  color: #0747dd;
  flex: 1;
  font-size: 1.2rem;
  cursor: pointer;
  transition: 0.2s;
  min-width: 160px;
  background: #ededed;

  &:hover {
    background: #2e6cfa;
    color: #eee;
  }
  &.btn-active {
    background: #2e6cfa;
    color: #eee;
  }
}
.primary-btn {
  background: #0747dd;
  color: #eee;
}
.cards-container {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  margin-top: -10px;

  &.available {
    border-top: 3px solid #eee;
    padding-top: 1rem;
  }

  .card {
    background: #ededed;
    padding: 0.8rem 2rem;
    border-radius: 50px;
    flex: 1;
    text-align: center;
    color: #0747dd;
    font-weight: bold;
    min-width: 160px;
    position: relative;

    b {
      font-size: 14px;
      position: absolute;
      color: #eee;
      top: 50%;
      transform: translateY(-50%);
      right: 10px;
      cursor: pointer;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background: crimson;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}
</style>
