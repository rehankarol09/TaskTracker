<script setup lang="ts">
import { ref } from "vue";
interface IProps {
  id: string;
  title: string;
  description: string;
}
const props = defineProps<{ updateTask: IProps }>();

const task = ref({ ...props.updateTask });

const emit = defineEmits<{
  (e: "close:Modal"): void;
  (e: "load:task"): void;
}>();
const closeModal = () => {
  emit("close:Modal");
};
const saveData = () => {
  const taskId = task.value.id;
  if(taskId){
    const tasks = JSON.parse(localStorage.getItem("tasks") || "[]");
    const taskIndex = tasks.findIndex(taskItem => taskItem.id = task.value.id );
    if(taskIndex != -1){
      tasks[taskIndex] = task.value;
      localStorage.setItem("tasks", JSON.stringify(tasks));
    }
    console.log(tasks)

  }
  emit("load:task")
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



          <!-- Button for saving tags -->
          <button
            type="submit"
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          >
            Save Changes
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
