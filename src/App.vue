<script setup>
import { ref } from 'vue'

// Коллекции для нижних блоков
const userItems = ref([
  { id: 1, name: "Shoes 1" },
  { id: 2, name: "Shoes 2" },
  { id: 3, name: "Shoes 3" },
  { id: 4, name: "Shoes 4" },
  { id: 5, name: "T-shirt 1" },
  { id: 6, name: "T-shirt 2" },
  { id: 7, name: "T-shirt 3" },
  { id: 8, name: "T-shirt 4" }
])

const choiceItems = ref([
  { id: 11, name: "Jacket 1" },
  { id: 12, name: "Jacket 2" },
  { id: 13, name: "Jacket 3" },
  { id: 14, name: "Jacket 4" },
  { id: 15, name: "Hoodie 1" },
  { id: 16, name: "Hoodie 2" },
  { id: 17, name: "Hoodie 3" },
  { id: 18, name: "Hoodie 4" }
])

// Выбранные элементы
const selectedUserItems = ref([])
const selectedChoiceItem = ref(null)

// Функции для выбора элементов
const toggleSelectUserItem = (item) => {
  const index = selectedUserItems.value.findIndex(i => i.id === item.id)
  
  if (index >= 0) {
    // Если элемент уже выбран, удаляем его
    selectedUserItems.value.splice(index, 1)
  } else {
    // Если элемент не выбран и выбрано менее 6 элементов, добавляем его
    if (selectedUserItems.value.length < 6) {
      selectedUserItems.value.push(item)
    }
  }
}

const selectChoiceItem = (item) => {
  selectedChoiceItem.value = item
}

// Проверка, выбран ли элемент
const isUserItemSelected = (item) => {
  return selectedUserItems.value.some(i => i.id === item.id)
}

const isChoiceItemSelected = (item) => {
  return selectedChoiceItem.value && selectedChoiceItem.value.id === item.id
}
</script>

<template>
  <div class="container">
    <!-- Верхний ряд -->
    <div class="top-row">
      <!-- Верхний левый блок - выбранные вещи пользователя -->
      <div class="selected-user-items">
        <div class="items-grid">
          <div v-for="item in selectedUserItems" :key="item.id" class="item">
            {{ item.name }}
          </div>
        </div>
        <div class="selected-count">
          selected: {{ selectedUserItems.length }} / 6
        </div>
      </div>
      
      <!-- Верхний правый блок - выбранная вещь из вещей на выбор -->
      <div class="selected-choice-item">
        <div v-if="selectedChoiceItem" class="selected-item">
          {{ selectedChoiceItem.name }}
        </div>
        <div v-else class="selected-item">
          SELECTED ITEM
        </div>
      </div>
    </div>
    
    <!-- Нижний ряд -->
    <div class="bottom-row">
      <!-- Нижний левый блок - вещи пользователя -->
      <div class="user-items">
        <div class="items-grid">
          <div 
            v-for="item in userItems" 
            :key="item.id" 
            class="item" 
            :class="{ 'selected': isUserItemSelected(item) }"
            @click="toggleSelectUserItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
      </div>
      
      <!-- Нижний правый блок - вещи на выбор -->
      <div class="choice-items">
        <div class="items-grid">
          <div 
            v-for="item in choiceItems" 
            :key="item.id" 
            class="item" 
            :class="{ 'selected': isChoiceItemSelected(item) }"
            @click="selectChoiceItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.top-row, .bottom-row {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.selected-user-items, .selected-choice-item, .user-items, .choice-items {
  border: 2px solid #000;
  padding: 15px;
  flex: 1;
}

.items-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.item {
  border: 2px solid #000;
  padding: 10px;
  min-width: 80px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.selected {
  background-color: #e0e0e0;
}

.selected-item {
  font-size: 24px;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.selected-count {
  margin-top: 20px;
}
</style>
