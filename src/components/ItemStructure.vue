<script setup>
import { ref } from 'vue'

const itemName = ref()
const newItemName = ref()
const isAddNew = ref(false)
const isNewFolder = ref(false)
const newType = ref()

// eslint-disable-next-line vue/valid-define-props, @typescript-eslint/no-unused-vars
const props = defineProps({
  item: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['removeItem'])

// Add new Item (File/Folder)
const addNewItem = () => {
  isAddNew.value = !isAddNew.value
  isNewFolder.value = false
}

const createNewItem = (type) => {
  isAddNew.value = false
  isNewFolder.value = true
  newType.value = type
}

const createNewFolder = (value) => {
  const newItem = {
    name: newItemName.value,
    type: newType.value
  }
  if (newType.value === 'folder') newItem.children = []
  value.push(newItem)

  newItemName.value = null
  isNewFolder.value = false
  newType.value = null
}

const cancelCreation = () => {
  newItemName.value = null
  isNewFolder.value = false
}

// Remove Item

const removeItem = () => {
  emit('removeItem')
}

const removeChildItem = (value, index) => {
  value.splice(index, 1)
}
</script>

<template>
  <li>
    <div class="item-name">
      <span> {{ item.type === 'folder' ? 'Folder : ' : 'File : ' }} {{ item.name }}</span>
      <div class="action-btn">
        <button v-if="item.type === 'folder'" class="add-btn" @click="addNewItem">+</button>
        <button class="cancel-btn" @click="removeItem()">-</button>
      </div>
    </div>
    <ul v-if="item.children && item.children.length">
      <ItemStructure
        v-for="(child, i) in item.children"
        :key="i"
        :item="child"
        @removeItem="removeChildItem(item.children, i)"
      />
    </ul>
  </li>
  <div v-if="isAddNew" class="create-box">
    <button class="item-btn" @click="createNewItem('folder')">Folder</button>
    <button class="item-btn" @click="createNewItem('file')">File</button>
  </div>
  <div v-if="isNewFolder" class="create-box">
    <p>{{ newType === 'folder' ? 'Folder : ' : 'File : ' }}</p>
    <input type="text" v-model="newItemName" />
    <button class="add-btn" @click="createNewFolder(item.children)">Add</button>
    <button class="cancel-btn" @click="cancelCreation">Cancel</button>
  </div>
</template>

<style scoped>
.item-name {
  display: flex;
  align-items: center;
  gap: 8px;
}

.item-name .action-btn {
  display: none;
}

.item-name:hover .action-btn {
  display: flex;
  align-items: center;
  gap: 4px;
}
</style>
