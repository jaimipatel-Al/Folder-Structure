<script setup>
import ItemStructure from './ItemStructure.vue'
import { ref } from 'vue'

const items = ref([])
const newFolderName = ref()
const isNewFolder = ref(false)

// add new folder
const addNewFolder = () => {
  isNewFolder.value = true
}

const createNewFolder = () => {
  if (newFolderName.value && newFolderName.value.trim() !== '') {
    items.value.push({
      name: newFolderName.value,
      type: 'folder',
      children: []
    })

    newFolderName.value = null
    isNewFolder.value = false
  }
}

const cancelCreation = () => {
  newFolderName.value = null
  isNewFolder.value = false
}

const removeItem = (value, index) => {
  value.splice(index, 1)
}
</script>


<template>
  <div class="folder-structure">
    <h1 class="title">FOLDER STRUCTURE</h1>
    <div>
      <button class="add-folder-btn" @click="addNewFolder()">Add folder to root</button>
      <ul>
        <ItemStructure
          v-for="(item, i) in items"
          :key="i"
          :item="item"
          @removeItem="removeItem(items, i)"
        />
      </ul>
      <!-- Add new folder  -->
      <div v-if="isNewFolder" class="create-box">
        <p>Folder :</p>
        <input type="text" v-model="newFolderName" />
        <button class="add-btn" @click="createNewFolder">Add</button>
        <button class="cancel-btn" @click="cancelCreation">Cancel</button>
      </div>
    </div>
  </div>
</template>