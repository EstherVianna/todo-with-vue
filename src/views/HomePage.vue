<script setup>
import {reactive} from 'vue'
import TaskPage from '../components/TaskPage.vue';

const state = reactive({
    task: '',
    tasks: [],
    renderTasks: false,
    hasTask: true
})

const renderList = ()=> state.renderTasks = state.tasks.length > 0

function handleAddTask(){
    if (state.task.trim() !== '') {
        state.tasks.push(state.task)
        state.task = ''
        renderList()
        state.hasTask = true
        
    }else{
        state.hasTask = false
    }
}

const deleteTask = (index)=>{
    state.tasks.shift(index)
}

</script>

<template>
<div class="container">
    <h1>Todo List</h1>
    <form>
        <div class="container-content">
            <input v-model="state.task" type="text" class="container-content-input">
            <button @click.prevent="handleAddTask" :class="active" type="button">
                <span class="material-symbols-outlined">add</span>
            </button>
        </div>
    </form>
    <p v-if="state.hasTask === false"> Please, enter a task!</p>
        <ul class="task-list" v-if="state.renderTasks">
            <TaskPage :tasks-list="state.tasks" :task-item="state.task" @delete-task="deleteTask()"/>
        </ul>
</div>
</template>

<style scoped lang="sass">
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