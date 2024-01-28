<script setup lang="ts">
import TaskList from "./views/TaskList.vue";
import {useDark,useToggle} from "@vueuse/core";
const isDark = useDark();
const toggleMode = useToggle(isDark)
</script>

<template>
  <div class="h-screen">
    <TaskList/>
  </div>
</template>

<style>
.dark{
  color:#fff;
  background: #16171d;
}
</style>
