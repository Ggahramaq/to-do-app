<script setup>
import { ref } from 'vue';

const tasks = ref([])
const toggleStatus = ref({
    // 1: false
})

const newTask = ref('');

const toggleTruncation = (index) => {
    toggleStatus.value[index] = !toggleStatus.value[index]
}

const truncatedDescription = (index) => {
  let description = tasks.value[index-1];
  if (!toggleStatus.value[index]) {
    description = description.substring(0, 20) + '...';
  }
  return description;
}; 

const addTask = () => {
    if (newTask.value.trim() !== '') {
        tasks.value.push(newTask.value)
        newTask.value = '';
        toggleStatus.value[tasks.value.length] = false;
    }
};

const deleteTask = (index) => {
    tasks.value.splice(index, 1);
    delete toggleStatus.value[index+1];
    const newTogglestatus = {};
    let count = 1;
    for (let key in toggleStatus.value) {
        newTogglestatus[count] = toggleStatus.value[key]
        count++
    }
    toggleStatus.value = newTogglestatus;
}

</script>

<template>
    <div class="flex justify-center items-center h-screen">
        <div class="bg-white w-130 border rounded-xl">
        <div class="text-left text-4xl mt-7 ml-8">To-Do List 📑</div>
        <div class="">
            <form @submit.prevent="addTask" class="flex ml-7 mt-5 bg-zinc-200 w-110 h-15 border-0 rounded-4xl">
                <input type="text" placeholder="Add your task" id="newTask" v-model="newTask" class="w-100 ml-3" />
                <button type="submit" class="bg-orange-500 cursor-pointer border-0 rounded-4xl w-50 items-center">Add</button>
            </form>
        <div class="text-center text-2xl mt-7">To do:</div>
        <div class="w-full mt-4 flex-grow overflow-y-auto max-h-[250px]">
            <ul class="w-full mt-4 flex flex-col gap-2">
                <li v-for="(task, index) in tasks" :key="task" class="flex justify-between p-2 rounded-lg w-full">
                <!-- TODO: add Less/ More logic -->
                <span class="text-3xl w-87 break-words">
                    <label>
                        <input type="checkbox" checked="checked">
                    </label>
                    <div v-if="task.length<20">{{index+1}}.{{ task }}</div>
                    <div v-else>
                        {{ index+1 }}.{{truncatedDescription(index+1)}}
                        <button @click="toggleTruncation(index+1)" class="text-green-500 transition duration-300 hover:text-green-600 cursor-pointer">{{ toggleStatus[index+1] ? "Less" : "More" }}</button>
                        <!-- TODO: add toggle logic -->
                    </div>
                </span>
                <button @click="deleteTask(index)" class="text-red-500 text-3xl cursor-pointer duration-300 ease-out hover:text-red-700">X</button>
                </li>
            </ul>
        </div>
        </div>
    </div>
    </div>
</template>