<!-- The difference between Task.vue and Tasks.vue is that Tasks.vue will iterate through all the tasks data, and populate one Task component for each task data.
This particular file is the single task UI-->
<template>
    <!-- What the :class is doing is to have dynamic/conditional class
    Here, task.reminder will either return true or false. (if it is true, we want to add the reminder class to this div, else it's empty — this is just a ternary operator)
    Only for task with class = reminder will we apply some style - in this case, it's just a green border -->
    <!-- We are also adding a double click action to toggle the reminder (the border of task will toggle between transparent to green, etc.)
    You'll also realize the @dblclick emit is more straightforward than the h3's @click — they are doing the same thing: both are emitting.
    Only caveat is the h3's method is more troublesome because you need to define the method in the script section -->
    <div
        @dblclick="$emit('toggle-reminder', task.id)"
        :class="['task', task.reminder ? 'reminder' : '']"
    >
        <h3>
            {{ task.text }}
            <i @click="onDelete(task.id)" class="fas fa-times"></i>
        </h3>
        <p>{{ task.day }}</p>
    </div>
</template>

<script>
export default {
    name: "Task",
    props: {
        // task is an object that contain information for one particular task.
        task: Object,
    },
    methods: {
        onDelete(id) {
            // You want to emit/push up the method to the root component (App.vue).
            // The convention is to name the action in kebab case, i.e. some-action
            // After emitting this up, the next level that imported this Task.vue component would be Tasks.vue
            // So you'd want to catch it in the Tasks.vue component
            this.$emit("delete-task", id);
        },
    },
};
</script>

<style scope>
.fas {
    color: red;
}
.task {
    background: #f4f4f4;
    margin: 5px;
    padding: 10px 20px;
    cursor: pointer;
}
.task.reminder {
    border-left: 5px solid green;
}
.task h3 {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>
