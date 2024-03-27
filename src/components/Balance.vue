<script setup lang="ts">
import { defineProps } from 'vue';

const props = defineProps({
    total: {
        type: Number,
        required: true,
    },
});
</script>
<template>
    <h4>Final Balance</h4>
    <h1 id="balance">KD {{ total }}</h1>
</template>