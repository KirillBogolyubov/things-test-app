<template>
  <div class="w-75 ma-0 ma-auto">
    <div class="d-flex ga-5">
      <div class="w-50 border-solid mb-2 pa-2">
        <div class="d-flex flex-wrap ga-2 mb-2">
          <div class="clothes-block__item" v-for="(item, index) in selectedClothes" :key="index" @click="removeSelectedClothes(item)">{{item.name}}</div>
        </div>
        Выбрано: {{selectedClothes.length}} / 6 <button class="cursor-pointer border-solid pa-1 ml-2" @click="removeAllSelectedClothes">Очистить</button>
      </div>

      <div class="w-50 border-solid mb-2 pa-2">
        <div class="d-flex flex-wrap ga-2">
          <div v-if="selectedOutwear" class="clothes-block__item" @click="removeSelectedOutwear">{{selectedOutwear.name}}</div>
        </div>
      </div>
    </div>

    <div class="d-flex justify-space-between ga-5">
      <div class="clothes-block">
        <div class="clothes-block__item" v-for="(item, index) in clothes" :key="index" @click="selectClothes(item)">
          {{item.name}}
        </div>
      </div>

      <div class="clothes-block">
        <div class="clothes-block__item" v-for="(item, index) in outwear" :key="index" @click="selectOutwear(item)">
          {{item.name}}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import {ref} from "vue";
import clothes from "@/collections/clothes";
import outwear from "@/collections/outwear";

const selectedClothes = ref([])
const selectedOutwear = ref(null)

// Добавление вещи в блок выбранных вещей
const selectClothes = (item) => {
  if(selectedClothes.value.length === 6) {
    return
  }
  const [selectedItem] = clothes.value.splice(item, 1)
  selectedClothes.value.push(selectedItem)
}

// Удаление вещи при нажатии на нее
const removeSelectedClothes = (item) => {
  const [selectedItem] = selectedClothes.value.splice(item, 1)
  clothes.value.push(selectedItem)
  clothes.value.sort((a, b) => a.id - b.id)
}

// Удаление всех выбранных вещей
const removeAllSelectedClothes = () => {
  selectedClothes.value.forEach(el => {
    clothes.value.push(el)
    clothes.value.sort((a, b) => a.id - b.id)
  })
  selectedClothes.value = []
}

const selectOutwear = (item) => {
  if (selectedOutwear.value) {
    outwear.value.push(selectedOutwear.value);
  }
  const index = outwear.value.findIndex(i => i.id === item.id);
  if (index !== -1) {
    outwear.value.splice(index, 1);
  }
  selectedOutwear.value = item;
  outwear.value.sort((a, b) => a.id - b.id);
};

const removeSelectedOutwear = () => {
  outwear.value.push(selectedOutwear.value)
  outwear.value.sort((a, b) => a.id - b.id);
  selectedOutwear.value = null
}
</script>

<style lang="scss">
.clothes-block {
  display: flex;
  flex-wrap: wrap;
  padding: 20px;
  border: 1px solid black;
  row-gap: 20px;
  column-gap: 20px;
  width: 50%;

  &__item {
    cursor: pointer;
    padding: 20px;
    border: 1px solid black;
    height: fit-content;
  }
}
</style>