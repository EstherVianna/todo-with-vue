<script setup>
import {reactive, computed} from 'vue'
import TaskPage from '../components/TaskPage.vue';

const state = reactive({
    task: '',
    tasks: [],
    hasTask: true,
    totalTasks: 0,
    completeTasks: 0
})

const handleAddTask = ()=>{
    if (state.task.trim() !== '') {
        state.tasks.push({
      name: state.task,
      complete: false,
      contentIsEditable: true
    });
    state.task = ''
    state.hasTask = true
    ++state.totalTasks
    }else{
            state.hasTask = false
        }
}

const handleCompleteTask = (task)=>{
    if(task.complete){
        ++state.completeTasks
        task.contentIsEditable = false
    }else{
        --state.completeTasks
        task.contentIsEditable = true
    }
}

const deleteTask = (index)=>{
    state.tasks.splice(index,1)
    --state.totalTasks
    --state.completeTasks
}

</script>
<template>
<div class="container">
    <div class="header">
        <h1 class="header-title">Todos</h1>
        <div class="header-subtitle">
            <h2 v-if="state.totalTasks > 0"> {{ state.totalTasks }} <span class="header-subtitle-total">Total</span></h2>
            <h2 v-if="state.completeTasks > 0"> {{ state.completeTasks }} <span class="header-subtitle-total">Complete</span></h2>
        </div>
    </div>    
    <form>
        <div class="container-content">
            <input type="text" class="container-content-input" v-model="state.task" @keypress.enter.prevent="handleAddTask">
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
            :tasks-list="state.tasks"
            @add-complete-task="handleCompleteTask(task)"
            @delete-task="deleteTask(index)"/>
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
        flex-flow: column nowrap
        width: 100%
        height: 100%
        font-size: 20px
        &-total
            font-size: 12px
            padding:   15px 0px 0px 4px
.container
    display: flex
    align-items: center
    flex-flow: column nowrap
    margin: auto
    max-width: 70%
    height: 100vh
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

</style>