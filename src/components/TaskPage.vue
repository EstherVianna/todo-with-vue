<script setup>

defineProps({
    taskItem: String,
    tasksList: Array
}
)

defineEmits(['deleteTask'])

</script>
<template>
    <li v-for="task, index in tasksList" :key="index" class="task-item">
        <input type="checkbox" v-model="task.completed" class="task-item-check"/>
          <span> {{task}} </span>
        <button @click="$emit('deleteTask', index)" class="task-deleteBtn">X</button>
        <!-- <button v-on:click= "EditItem"
          class="button btn-delete"></button> -->
    </li>
</template>

<style scoped lang="sass">
.task-item
    display: flex
    align-items: center
    justify-content: space-between
    flex-flow: row wrap
    gap: 50px
    border: 1.5px solid #ff0039
    margin: 10px 0px
    min-width: 400px
    line-height: 35px
    font-size: 16px
   
    &-check
        margin: 5px
        cursor: pointer

</style>