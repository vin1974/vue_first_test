<template>
    <div 
        @dblclick="$emit('toggle-reminder', task.id)"
        v-bind:class="[task.reminder ? 'reminder' : '', 'task']">
        <h4>{{task.text}}
            <i @click="$emit('delete-task',task.id)"
                class="fas fa-times"></i>
        </h4>
        <p>{{task.day}}</p>

    </div>
</template>

<script>
    export default {
        name: "Task",
        components: {

        },
        props: {
            task: Object
        },
        // methods: {
        //     onDelete(id){
        //         this.$emit('delete-task', id);
        //     }
        // }
    }
</script>

<style scoped>
    .fas {
        color: red;
    }
    .task {
        background: #fce1c3;
        margin: 5px;
        padding: 10px 20px;
        cursor: pointer;
        text-align: left;
    }

    .task.reminder {
        border-left: 5px solid green;
    }

    .task h4 {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding-bottom: 4px;
    }

</style>