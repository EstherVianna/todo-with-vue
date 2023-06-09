<script setup>
import {ref} from 'vue'

defineProps({
  task: Object
});

const labelRef = ref(null);
const contentIsEditable = true;

const focusLabel= () => {
    labelRef.value.focus();
}

</script>
<template>
<li class="task-item">
    <input type="checkbox" id="checkbox" class="task-item-check" v-model="task.complete" @change="handleTaskCompleted, $emit('addCompleteTask')"/>
        <label class="task-item-label"
        contenteditable="true" 
        ref="labelRef"
        :class="{ 'task-item-completed':task.complete }">
         {{task.name}} 
        </label>
    <div class="task-item-btn">
        <button @click="$emit('deleteTask')" >
            <span class="material-symbols-outlined">delete</span>
        </button>
        <button>
            <span class="material-symbols-outlined" @click.prevent="focusLabel">edit</span>
        </button>
    </div> 
</li>
</template>

<style scoped lang="sass">
.task-item
    display: flex
    align-items: center
    justify-content: space-between
    flex-flow: row wrap
    gap: 50px
    border-radius: 5px
    margin: 10px 0px
    width: clamp(200px, 400px, 600px)
    font-size: 16px
    background: #30303033
    label
        cursor: pointer
        max-width: 55%
        height: fit-content
        line-height: 35px
        &:focus
            border-radius: 2.5px
            background: #f28f77
            outline: none
            caret-color: #fff
    &-check
        margin: 5px
        cursor: pointer
        &checked > span
            text-decoration: line-through
    &-completed
        text-decoration: line-through
    &-btn
        display: flex
        align-items: center
        gap: 3px
        margin-right: 3px
        button
            border: none
            background: transparent
            cursor: pointer
            svg
                font-size: 12px

</style>