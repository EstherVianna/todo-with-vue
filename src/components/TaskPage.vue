<script setup>
import {reactive} from 'vue'

defineProps({
    taskItem: String,
    tasksList: Array
}
)

defineEmits(['deleteTask'])

</script>
<template>
<li v-for="task, index in tasksList" :key="index" class="task-item">
    <input type="checkbox" v-model="task.checked" class="task-item-check" id="checkbox"/>
        <label for="checkbox"> {{task}} </label>
    <button @click="$emit('deleteTask', index)" class="task-item-remove">
        <span class="material-symbols-outlined">delete</span>
    </button>
</li>
</template>

<style scoped lang="sass">
.task-item
    display: flex
    align-items: center
    justify-content: space-between
    flex-flow: row wrap
    gap: 50px
    border: 2px solid #ff0039
    margin: 10px 0px
    min-width: 400px
    line-height: 35px
    font-size: 16px
   
    &-check
        margin: 5px
        cursor: pointer
        &checked > span
            text-decoration: line-through


    &-remove
        border: none
        background: transparent
        cursor: pointer
        svg
            font-size: 12px


</style>