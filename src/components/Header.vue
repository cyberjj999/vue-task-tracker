<template>
    <header>
        <h1>{{ title }}</h1>
        <!-- <h1>Task Tracker</h1> -->
        <!-- Emit the toggle-add-task action from Button.vue to Header.vue to App.vue -->
        <Button
            @btn-click="$emit('toggle-add-task')"
            :text="showAddTaskInHeader ? 'Close' : 'Add Task'"
            :color="showAddTaskInHeader ? 'red' : 'green'"
        />
    </header>
</template>

<script>
// Just like how you can import `Header.vue` into `App.vue` (the root component), you can also import other components into `Header.vue` (i.e. you can have components under components)

// The step I've seen so far is
// 1. Create a `<Something>.vue` — populate them with the 3 fields, "template", "script", and "style" (style is optional)
// 2. You import this `<Something>.vue` (i.e. Navbar.vue) into either a root component of some other component (just like the line here)
// 3. The last thing you need to do is to "export default the component" just like the lines below
import Button from "./Button.vue";

export default {
    name: "Header",
    // Option 1: You can define props as an array
    // props: ["title"],

    // Option 2: You can also define props as an object
    props: {
        // Option 2a: You can even specify the datatype associated with each prop
        // title: String,

        // Option 2b: or better still, define the individual prop as an object to set a default value
        title: {
            type: String,
            default: "Task Tracker",
        },
        // either true/false depending on whether user wants to show the "AddTask" UI or not
        showAddTaskInHeader: Boolean,
    },
    components: {
        Button,
    },
};
</script>

<!-- 'scoped' in here means that the style will only apply to this particular component, i..e this is the Header.vue file so it's the Header component that will have this style.  -->
<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
</style>
