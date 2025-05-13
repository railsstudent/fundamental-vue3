<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { ref, computed } from 'vue'

type Item = { id: number; label: string; highPriority: boolean; purchased: boolean }

const header = ref('Shopping List App')
const items = ref<Item[]>([])
const reverse_items = computed(() => [...items.value].reverse())
const num_items_purchased = computed(() =>
  items.value.reduce((acc, item) => acc + (item.purchased ? 1 : 0), 0),
)
const num_items_purchased_label = computed(()=> {
  const unit = num_items_purchased.value === 1 ? 'item' : 'items'
  return `${num_items_purchased.value} ${unit} purchased`
})

const newItem = ref('')
const newItemHighPriority = ref(false)

const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    highPriority: newItemHighPriority.value,
    purchased: false,
  })
  newItem.value = ''
  newItemHighPriority.value = false
}

const isEditing = ref(false)

const toggleEditing = (value: boolean) => {
  isEditing.value = value
  newItem.value = ''
  newItemHighPriority.value = false
}

const togglePurchase = (item: Item) => {
  item.purchased = !item.purchased
}

const deleteItem = (id: number) => {
  items.value = items.value.filter((item) => item.id !== id)
}
</script>

<template>
  <div class="header">
    <h1>{{ header || 'Welcome' }}</h1>
    <button class="btn" v-if="isEditing" @click="toggleEditing(false)">Cancel</button>
    <button class="btn btn-primary" v-else @click="toggleEditing(true)">Add Item</button>
  </div>
  <form class="add-item-form" v-if="isEditing" @submit.prevent="saveItem">
    <input v-model.trim="newItem" placeholder="Add new item" />
    <label>
      <input type="checkbox" v-model="newItemHighPriority" />
      <span :style="{ 'font-weight': newItemHighPriority ? 'bold' : 'normal' }">
        High Priority</span
      >
    </label>
    <button class="btn btn-primary" :disabled="newItem.length < 5">Save Item</button>
  </form>
  <ul v-if="items.length > 0">
    <div class="header">
      <p>{{  num_items_purchased_label }}</p>
    </div>
    <div class="list-item" v-for="item in reverse_items" :key="item.id">
      <li
        :class="[{ priority: item.highPriority }, { strikeout: item.purchased }]"
        @click="togglePurchase(item)"
      >
        {{ item.id }} - {{ item.label }}
      </li>
      <button v-if="!item.purchased" class="btn btn-cancel" aria-label="Delete" @click="deleteItem(item.id)">
        <Icon icon="ic:baseline-remove" />
      </button>
    </div>
  </ul>
  <p v-else>Nothing to see here.</p>
</template>

<style scoped>
input {
  padding: 0.25rem;
}

div.list-item {
  display: flex;
}

div.list-item > li {
  margin-right: 0.5rem;
}
</style>
