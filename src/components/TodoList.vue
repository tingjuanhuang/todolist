<template>
    <div class="toDoList">
        <div class="imgBox">
          <div class="copyright">Copyright © 2024 TingJuanHuang. All rights reserved.</div>
        </div>
        <div class="toDoListBox">
            <div class="inputContainer">
                <input v-model="newTask" type="text" placeholder="新增待辦事項">
                <button class="addItem" @click="addTask"></button>
            </div>
            <ul>
                <li v-for="(task, index) in tasks" :key="index" :class="{ completed: task.completed }">
                    <label class="checkBtn">
                        <input type="checkbox" v-model="task.completed">
                        <span class="checkmark"></span>
                    </label>
                    <span>{{ task.text }}</span>
                    <button class="deleteBtn" @click="deleteTask(index)"></button>
                </li>
            </ul>
            <div class="itemStatus">
                <p>待完成項目：{{ unfinishedTaskCount }} 個</p>
                <button class="clearCompleted" @click="clearCompletedTasks">清除已完成項目</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const newTask = ref('')
const tasks = ref(JSON.parse(localStorage.getItem('tasks')) || [])

const addTask = () => {
  tasks.value.push(
    { text: `${newTask.value}`, completed: false }
  )
  newTask.value = ''
  saveTasks()
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
  saveTasks()
}

const clearCompletedTasks = () => {
  tasks.value = tasks.value.filter(task => !task.completed)
  saveTasks()
}

const unfinishedTaskCount = computed(() => {
  return tasks.value.filter(task => !task.completed).length
})

const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

watch(tasks, saveTasks, { deep: true })
</script>
