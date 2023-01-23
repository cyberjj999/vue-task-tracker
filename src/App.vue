<template>
    <div class="container">
        <!-- Switch showAddTask to the opposite of it (i.e. toggling) -->
        <Header
            @toggle-add-task="this.showAddTask = !this.showAddTask"
            :showAddTaskInHeader="showAddTask"
            title="Task Tracker App"
        />
        <!-- For conditional rendering, i.e. only show this HTML element if <value> is true, you can either use "v-if" or "v-show" 
        Generally speaking, v-if has higher toggle costs while v-show has higher initial render costs. So prefer v-show if you need to toggle something very often, and prefer v-if if the condition is unlikely to change at runtime.-->
        <div v-show="showAddTask">
            <AddTask @add-task="addTask" />
        </div>

        <!-- We want to dynamically generate the tasks 
        So we set the tasks attribute for the Tasks component with some value(i.e. <Tasks :tasks="myvalue")
        But here, we are not passing in random value, we're passing in what is defined in the data object below: an array of different tasks.-->

        <!-- The @delete-task is just a propagating/emission of "delete-task" action: see Task.vue and Tasks.vue for explanation.
        The logic is the same for @toggleRemidnder tbh-->
        <Tasks
            @toggle-reminder="toggleReminder"
            @delete-task="deleteTask"
            :tasks="tasks"
        />
    </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

export default {
    name: "App",
    components: { Header, Tasks, AddTask },

    // Two key lessons about data here:
    // 1. Usually we would have data from a backend api, but for this demo, we will define it inside the App inself.
    // We will, however, be using a JSON Server (fake api server) to stimulate api request in the later part of the tutorial.
    // 2. Usually you have VueX for state management, but we are going to define the data in the root component (App.vue) so we can pass this data to every other component (i.e. we cannot define this data object elsewhere because otherwise we can't pass it to every other component - at least not without vue x)
    // data is also a function that returns some object.
    data() {
        return {
            // Theoretically we could define the tasks here, but we will follow the React life cycle method (i.e. we can run specific code at specific time while each component is loaded)
            // Here, we will do something when this component is created.
            tasks: [],
            // In the UI, you'll have an "Add Task" button that will show/hide the "AddTask" form
            // We set it to be false at default - user has to click on "Add Task" for the form to be displayed
            showAddTask: false,
        };
    },
    // This is the created part of the lifecycle (i.e. when the component finally loads)
    // Usually at this point, the moment your component is just created, you will fetch your data through some http request - we'll just hardcode for demo purposes
    created() {
        this.tasks = [
            {
                id: 1,
                text: "Doctors Appointment",
                day: "March 1st at 2.30pm",
                reminder: true,
            },
            {
                id: 2,
                text: "School Meeting",
                day: "March 4th at 10pm",
                reminder: false,
            },
            {
                id: 3,
                text: "Harvard Admissions",
                day: "April 1 2023",
                reminder: false,
            },
        ];
    },
    methods: {
        addTask(newTask) {
            // This is very interesting, we see the spread syntax over here again ("..."), it simply set the this.task to be an array of current task (...tasks), then it appends the newTask.
            this.tasks = [...this.tasks, newTask];
        },
        deleteTask(id) {
            // Ask user before you confirm delete
            if (confirm("Confirm Delete?"))
                // Resetting the task data, filtering the task array to only contain task taht doesn't have the current id (i.e. deleting the task based on task ID)
                this.tasks = this.tasks.filter((task) => task.id !== id);
        },
        toggleReminder(id) {
            // It seems very complicated, but this is just using the array's map method to achieve one goal: toggle the reminder attribute
            // It loops through the tasks
            this.tasks = this.tasks.map((task) =>
                // and if the task matches the id
                task.id === id
                    ? {
                          // The ""...task" simply represents spreading across the original's task attribute (i.e. we copy every single attribute)
                          ...task,
                          // and we are merely changing 1 value, which is reminder in this case
                          reminder: !task.reminder,
                      }
                    : task
            );
        },
    },
};
</script>

<style>
/* These are your global styles 
Imported from Bradtraversy GitHub - https://github.com/bradtraversy/vue-crash-2021/blob/master/src/App.vue*/
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
body {
    font-family: "Poppins", sans-serif;
}
.container {
    max-width: 500px;
    margin: 30px auto;
    overflow: auto;
    min-height: 300px;
    border: 1px solid steelblue;
    padding: 30px;
    border-radius: 5px;
}
.btn {
    display: inline-block;
    background: #000;
    color: #fff;
    border: none;
    padding: 10px 20px;
    margin: 5px;
    border-radius: 5px;
    cursor: pointer;
    text-decoration: none;
    font-size: 15px;
    font-family: inherit;
}
.btn:focus {
    outline: none;
}
.btn:active {
    transform: scale(0.98);
}
.btn-block {
    display: block;
    width: 100%;
}
</style>
