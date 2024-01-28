<script setup lang="ts">
import { reactive } from "vue";
import { v4 as uuidv4 } from "uuid";

const emit = defineEmits<{
  (e: "close:Modal"): void;
  (e: "load:task"): void;
}>();
const task = reactive({
  title: "",
  description: "",
  tags: "",
});
const closeModal = () => {
  emit("close:Modal");
};
const saveData = () => {
  console.log(task.tags.length)
  const tagsString = task.tags.length > 0 ? task.tags.split(",") : [];

  const taskItem = {
    id: uuidv4(),
    createdAt: new Date(),
    status: "Pending",
    title: task.title,
    description: task.description,
    tags: tagsString,
  };

  const existingTasks = JSON.parse(localStorage.getItem("tasks") || "[]");

  existingTasks.push(taskItem);

  localStorage.setItem("tasks", JSON.stringify(existingTasks));
  console.log(existingTasks)
  emit("load:task");
  closeModal();
};
</script>
<template>
  <div
    id="myModal"
    class="modal fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center"
  >
    <!-- Modal content -->
    <div
      class="modal-content bg-white p-8 w-11/12 md:w-5/12 lg:w-4/12 rounded min-h-96 flex flex-col"
    >
      <span
        id="closeModal"
        class="close text-3xl font-bold self-end"
        @click="closeModal"
        >&times;</span
      >

      <div class="form-container" @submit.prevent="saveData">
        <h3 class="text-xl font-bold mb-4">Task Details</h3>
        <form class="flex flex-col">
          <input
            type="text"
            placeholder="Title"
            required
            class="mb-4 p-2 border rounded"
            v-model="task.title"
            :required="'true'"
          />
          <textarea
            placeholder="Enter Description"
            rows="5"
            class="mb-4 p-4 border rounded"
            v-model="task.description"
            :required="'true'"
          ></textarea>

          <!-- Tag input -->
          <input
            type="text"
            placeholder="Tags (comma-separated)"
            class="mb-4 p-2 border rounded"
            v-model.trim="task.tags"
          />

          <!-- Button for saving tags -->
          <button
            type="submit"
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          >
            Add
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
