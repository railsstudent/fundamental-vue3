<script setup lang="ts">
import { ref } from 'vue';

const header = ref('Shopping List App')
const items = ref<{ id: number, label: string }[]>([]);

const newItem = ref('');
const newItemHighPriority = ref(false);
// const icecreamFlavors = ref<string[]>([])

const saveItem = () => {
  items.value.push({ id: items.value.length + 1, label: newItem.value });
  newItem.value = '';
};

const isEditing = ref(false);

const toggleEditing = (value: boolean) => {
  isEditing.value = value;
  newItem.value = '';
};

</script>

<template>
  <div class="wrapper">
    <div class="header">
      <h1>{{ header || 'Welcome' }}</h1>
      <button class="btn" v-if="isEditing" @click="toggleEditing(false)">Cancel</button>
      <button class="btn btn-primary" v-else @click="toggleEditing(true)">Add Item</button>
    </div>
    <form class="add-item-form" v-if="isEditing" @submit.prevent="saveItem">
      <input v-model.trim="newItem"  placeholder="Add new item" />
      <label>
        <input type="checkbox" v-model="newItemHighPriority" />High Priority
      </label>
      <button class="btn btn-primary">Save Item</button>
    </form>    
    <ul v-if="items.length > 0">
      <li v-for="{ id, label } in items" :key="id">
        {{id}} - {{ label }}
      </li>
    </ul>
    <p v-else>Nothing to see here.</p>
    <!-- <br />
    <input type="checkbox" v-model="icecreamFlavors" value="vanilla" /> Vanilla
    <input type="checkbox" v-model="icecreamFlavors" value="chocolate chips" /> Cholocate Chips
    <input type="checkbox" v-model="icecreamFlavors" value="strawberry" /> Strawberry -->
  </div>
</template>

<style scoped>
  input {
    padding: 0.25rem;
  }
</style>
