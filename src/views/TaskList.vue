<script setup lang="ts">
import {ref,onMounted} from "vue";
import TaskCard from "../components/TaskCard.vue";
import AddModal from "../components/AddModal.vue";
import EditModal from "../components/EditModal.vue";

const tasks = ref();
const updateTask = ref();
const showAddModal = ref(false);
const showEditModal = ref(false);
const openAddModal=()=>{
    showAddModal.value = true;
}
const closeModal = () =>{
  showAddModal.value=false
}
const openEditModal=(taskItem)=>{
  showEditModal.value = true;
  updateTask.value = taskItem;
}
const closeEditModal = () =>{
  showEditModal.value=false
}
const loadTasks = () => {
  const taskList = JSON.parse(localStorage.getItem("tasks") || "[]");
  tasks.value = taskList;
};

// Load tasks on component mount
onMounted(() => {
  loadTasks();
});

// Watch for changes in local storage and update tasks accordingly
</script>
<template>
    <div class="flex justify-end w-full p-3">
        <button class="ml-2 px-3 py-1 bg-blue-500 text-white rounded" @click="openAddModal">Add</button>
    </div>
    <div class="flex flex-wrap w-9/12 mx-auto">
        <TaskCard
        v-for="task in tasks"
        :id="task.id"
        :title="task.title"
        :description="task.description"
        :tags="task.tags"
        :status="task.status"
        :createdAt="task.createdAt"
        @load:task="loadTasks"
        @open:edit="openEditModal(task)"
        />
    </div>
    <AddModal
      v-if="showAddModal"
      @close:Modal="closeModal"
      @load:task="loadTasks"
    />
    <EditModal
      v-if="showEditModal"
      @close:Modal="closeEditModal"
      @load:task="loadTasks"
      :updateTask="updateTask"
    />
</template>