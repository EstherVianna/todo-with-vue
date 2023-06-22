<script setup>
import { reactive, watch, onBeforeMount } from 'vue';
import TaskPage from '../components/TaskPage.vue';

const state = reactive({
  task: "",
  tasks: [],
  hasTask: true,
  totalTasks: 0,
  completeTasks: 0
});

const handleAddTask = () => {
  if (state.task.trim() !== "") {
    state.tasks.push({
      content: state.task,
      complete: false,
      contentEditable: true
    });
    state.task = "";
    state.hasTask = true;
    ++state.totalTasks;
  } else {
    state.hasTask = false;
  }};

watch(() => state.tasks, (newTask) => {
  localStorage.setItem('task', JSON.stringify(newTask))
}, { deep: true });

watch(() => state.completeTasks, () => {
  localStorage.setItem('completeTasks', state.completeTasks)
});

watch(() => state.totalTasks, () => {
  localStorage.setItem('totalTasks', state.totalTasks);
});

const handleCompleteTask = (task) => {
  task.contentEditable = !task.complete;

  if (task.complete) {
    ++state.completeTasks;
  } else {
    --state.completeTasks;
  }};

const deleteTask = (index, task) => {
  state.tasks.splice(index, 1);

  if(task.complete){
    --state.completeTasks;
    --state.totalTasks;
  }else{
    --state.totalTasks
  }};

onBeforeMount(() => {
  const storedTasks = localStorage.getItem('task');
  const storedCompleteTasks = localStorage.getItem('completeTasks');
  const storedTotalTasks = localStorage.getItem('totalTasks');

  if (storedTasks !== null) {
    state.tasks = JSON.parse(storedTasks)
    state.completeTasks = parseInt(storedCompleteTasks)
    if (storedCompleteTasks < 0) {
      storedCompleteTasks = 0;
    }
    state.totalTasks = storedTotalTasks
  }
});

</script>
<template>
<div class="container">
    <div class="header">
        <h1 class="header-title" >Todos</h1>
        <div class="header-subtitle">
            <h2 class="header-subtitle-task"
                v-if="state.totalTasks > 0">
             {{ state.totalTasks }}
             <span class="header-subtitle-task-num">Total</span></h2>
            <h2 class="header-subtitle-task"
                v-if="state.completeTasks > 0">
            {{ state.completeTasks }}
            <span class="header-subtitle-task-num">Complete</span></h2>
        </div>
    </div>    
    <form>
        <div class="container-content">
            <input type="text"
                   class="container-content-input"
                   v-model="state.task" 
                   @keypress.enter.prevent="handleAddTask">
            <button type="button" @click.prevent="handleAddTask">
                <span class="material-symbols-outlined rotate">add</span>
            </button>
        </div>
    </form>
    <p v-if="!state.hasTask"> Please, enter a task!</p>
    <ul class="task-list" v-if="state.tasks.length > 0">
            <TaskPage v-for="task, index in state.tasks" 
            :key="index" 
            :task="task"
            @add-complete-task="handleCompleteTask(task)"
            @delete-task="deleteTask(index, task)"/>
    </ul>
</div>
</template>
<style scoped lang="sass">
.header
    display: flex
    justify-content: space-evenly
    gap: 50%
    width: 40%
    margin-top: 10px
    &-subtitle
        display: flex
        flex-flow: row nowrap
        width: 20%
        gap: 12px
        &-task
            display: flex
            width: fit-content
            height: 100%
            &-num
                font-size: 12px
                padding:   10px 0px 0px 4px
.container
    display: flex
    align-items: center
    flex-flow: column nowrap
    margin: auto
    max-width: 70%
    min-height: 100vh
    height: 100%
    border-radius: 12px
    border: 1px solid #ffffff55
    backdrop-filter: blur(16px) saturate(180%)
    background-color: #ffffff66
    overflow: hidden
    
    &-content
        display: flex
        justify-content: center
        align-self: center
        gap: 12px
        margin: 20px 0
        width: 400px
        height: 50px
        &-input
            width: 90%
            height: 100%
            border: none
            border-bottom: 1px solid #fff
            padding-left: 5px
            font-size: 20px
            background-color: #ffffff33
            &:focus-visible
                border: none
                outline: 1px solid #fff
    button
        width: 10%
        height: 100%
        border: none
        font-size: 34px
        cursor: pointer
        color: #ff1493
        border-bottom: 1px solid #fff
        background-color: #ffffff33
        &:hover
            border-left: none
            border: 2px solid #fff
            opacity: 0.5
.task-list
    list-style-type: none
    padding: 10px
    display: flex
    flex-flow: column-reverse wrap

</style>