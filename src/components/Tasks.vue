<template>
    <div v-bind:key="item.id" v-for="item in tasks">
        <Task 
            @toggle-reminder="$emit('toggle-reminder', item.id)"
            @delete-task="$emit('delete-task', item.id)"
            :task="item" />
    </div>
</template>

<script>
    import Task from './Task'

   export default {
       name: 'Tasks',
       components: {
           Task
       },
       props: {
           tasks: Array
       },
       emits: ['delete-task', 'toggle-reminder']
   }
</script>


<style scoped>

</style>