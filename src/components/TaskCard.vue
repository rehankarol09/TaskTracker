<script setup lang="ts">
import { computed } from "vue";
interface IProps {
  id: string;
  createdAt: string;
  title: string;
  description: string;
  status: "Active" | "Completed";
  tags?: string[];
}
const props = defineProps<IProps>();
  const emit = defineEmits<{
    (e:"load:task"):void,
    (e:"open:edit"):void
}
>();
const truncateDescription = computed(() => {
  const words = props.description.split(" ");
  if (words.length > 30) {
    return words.slice(0, 30).join(" ") + "...";
  }
  return props.description;
});
function formatDate(inputDate: string) {
  const date = new Date(inputDate);

  const options = {
    year: "numeric",
    month: "long",
    day: "numeric",
  };

  return date.toLocaleString("en-US", options);
}
function deleteTask(id:string){
  const tasks = JSON.parse(localStorage.getItem("tasks") || "[]");
  const index = tasks.findIndex(task =>task.id === id);
  if(index != -1){
    tasks.splice(index,1);
    localStorage.setItem("tasks", JSON.stringify(tasks));
  }
  emit('load:task')
}
function openEditModal(){
  emit("open:edit")
}
</script>
<template>
  <div class="w-full rounded overflow-hidden shadow-lg" :data-task-id="props.id">
    <div class="px-6 py-4 md:flex md:justify-between">
      <div class="md:w-8/12">
        <div class="font-bold text-xl mb-2">{{ props.title }}</div>
        <p class="text-gray-700 text-base">
          {{ truncateDescription }}
        </p>
      </div>
      <div class="flex mt-4 md:mt-0 max-h-8">
        <button
          class="px-3 py-1  bg-green-500 text-white rounded"
          @click="deleteTask(props.id)"
        >
          Delete
        </button>
        <button
          class="ml-2 px-3 py-1  bg-blue-500 text-white rounded"
          @click="openEditModal"
        >
          Edit
        </button>
      </div>
    </div>

    <div class="px-6 pt-4 pb-2" v-if="tags">
      <span
        class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2"
        v-if="props && props?.tags.length > 0"
        v-for="tag in props.tags"
      >
        #{{ tag }}
      </span>
    </div>

    <div
      class="flex items-center justify-between px-6 pt-2 pb-2"
      v-if="props.status"
    >
      <div>
        <span
          class="inline-block bg-blue-200 rounded-full px-3 py-1 text-sm font-semibold text-blue-700"
        >
          {{ props.status }}
        </span>
      </div>
    </div>

    <div class="px-6 pt-2 pb-4" v-if="props.createdAt">
      <p class="text-gray-600 text-sm">{{ formatDate(props.createdAt) }}</p>
    </div>
  </div>
</template>

