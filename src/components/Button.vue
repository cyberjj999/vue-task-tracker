<template>
    <!-- We have a text and color prop here. We define the value in Header.vue by passing in some string value
    Reminder that if you want to change an attribute of an element, you need to use vbind, thus we use :style
    Also remember that vbind takes in prop in a different manner, not just two braces like {{ this }}-->
    <button @click="onClick()" :style="{ background: color }" class="btn">
        {{ text }}
    </button>
</template>

<script>
export default {
    name: "Button",
    props: {
        text: {
            type: String,
        },
        color: {
            type: String,
        },
    },
    // You can also define methods here to use it in your template tag.
    methods: {
        onClick() {},
    },
};
</script>
