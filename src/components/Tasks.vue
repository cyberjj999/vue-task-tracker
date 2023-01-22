<template>
    <!-- We also need a parent div because <template> doesn't accept a list rendering root element, i.e. your parent should be a single element, thus adding an additional div solves the problem.-->
    <div>
        <!-- Everything you have a list, i.e. when you're using v-for, you need to have a unique key v-bind to track each node's identity
        That's why we have :key="task.id"        
        - https://v2.vuejs.org/v2/guide/list.html#Maintaining-State
        -->
        <div v-for="task in tasks" :key="task.id">
            <!-- We could easily just add a h3 here (just like our commented code), but the TraversyMedia decided to create a single Task.vue component to make each UI even more granular (I'm not sure why but yes.)-->
            <!-- <h3>{{ task.text }}</h3> -->

            <!-- Referencing the Task component, the :task is just setting the prop data for Task component -->
            <!-- So earlier in our Task.vue, we are emitting up the "delete-task" action, i.e. propagating the event from a bottom component to the component above it (i.e. the component that imports it)
            This "Tasks" component is the middleman, the hierarchy is as such: Task.vue > Tasks.vue > App.vue 
            Here, in Tasks.vue, we are catching the "delete-task" action via @delete-task from Task.vue, and emitting the "delete-task" action up to App.vue.
            The same applies to @toggle-reminder -->
            <Task
                @toggle-reminder="$emit('toggle-reminder', task.id)"
                @delete-task="$emit('delete-task', task.id)"
                :task="task"
            />
        </div>
    </div>
</template>

<script>
import Task from "./Task";

export default {
    name: "Tasks",
    props: {
        // Tasks is specificed to be an array
        tasks: Array,
    },
    components: { Task },
    // apparently this is needed to get rid of the warning. I didn't need this line of code though - maybe the new version of vue doesn't have this constraint, but i'll just leave it here.
    emits: ["delete-task", "toggle-reminder"],
};
</script>
