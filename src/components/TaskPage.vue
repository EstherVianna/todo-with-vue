<script setup>
import {ref} from 'vue'

defineProps({
  task:{
    type: Object,
    required: true
  }
});

const labelRef = ref(null);

const focusLabel= () => {
    labelRef.value.focus();
};

</script>
<template>
<li class="task-item" draggable>
    <input type="checkbox" 
           class="task-item-check"
           v-model="task.complete"
           @change="$emit('add-complete-task', task)" />
       <label ref="labelRef">
            <input type="text"
                   class="task-item-input"
                   v-model="task.content"
                   :class="{ 'task-item-completed':task.complete }" />
        </label>
    <div class="task-item-btn">
        <button @click="$emit('deleteTask')" >
            <span class="material-symbols-outlined">delete</span>
        </button>
        <button v-show="task.contentEditable"
                @click.prevent="focusLabel">
            <span class="material-symbols-outlined">edit</span>
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
    gap: 10px
    border-radius: 5px
    margin: 5px 0px
    width: clamp(200px, 400px, 600px)
    font-size: 16px
    background: #30303033
    &-input
        line-height: 35px
        background: inherit
        border: none
        &:focus
            border-radius: 2.5px
            background: #f28f77
            outline: none
            caret-color: #fff
    &-check
        margin: 5px
        cursor: pointer
        > span
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