<template>
    <form @submit="onSubmit" class="add-form">
        <div class="form-control">
            <label>Task</label>
            <!-- v-model is used to bind a value here in <template> and also in our data properties. We typically use it in forms so the DOM input elements can modify our data property 
            Here, we are binding v-model to a field called "text"-->
            <input
                type="text"
                v-model="text"
                name="text"
                placeholder="Add Task"
            />
        </div>
        <!-- We're binding v-model to a field called day -->
        <div class="form-control">
            <label>Day & Time</label>
            <input
                type="text"
                v-model="day"
                name="day"
                placeholder="Add Day & Time"
            />
        </div>
        <div class="form-control form-control-check">
            <label>Set Reminder</label>
            <input type="checkbox" v-model="reminder" name="reminder" />
        </div>

        <input type="submit" value="Save Task" class="btn btn-block" />
    </form>
</template>

<script>
export default {
    name: "AddTask",
    // Each form field has some sort of data to hold, we are storing them here
    data() {
        return {
            text: "",
            day: "",
            reminder: false,
        };
    },
    methods: {
        // This runs on submit: i.e. when the user press "Add Task"
        onSubmit(e) {
            // Prevent default action of submit, i.e. won't make the page refresh or update when you click "Add Task"
            e.preventDefault();

            // Check for empty task, user cannot enter empty task
            if (!this.text) {
                alert("Task cannot be empty!");
                return;
            }
            // Create new task object
            const newTask = {
                // Generate a random ID up to 100,000 - just for testing purposes. In the real world you'll use an API with proper ID returned.
                id: Math.floor(Math.random() * 100000),
                text: this.text,
                day: this.day,
                reminder: this.reminder,
            };
            // Emit this add-task method upwards to App.vue (so App.vue can edit its data property to add a new task)
            this.$emit("add-task", newTask);
            // Reset the task object value (so the UI will reset also)
            this.text = "";
            this.day = "";
            this.reminder = false;
        },
    },
};
</script>

<style scoped>
.add-form {
    margin-bottom: 40px;
}
.form-control {
    margin: 20px 0;
}
.form-control label {
    display: block;
}
.form-control input {
    width: 100%;
    height: 40px;
    margin: 5px;
    padding: 3px 7px;
    font-size: 17px;
}
.form-control-check {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
.form-control-check label {
    flex: 1;
}
.form-control-check input {
    flex: 2;
    height: 20px;
}
</style>
