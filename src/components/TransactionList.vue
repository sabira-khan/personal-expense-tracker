<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

const props = defineProps<{
    transactions: {
        id: number;
        text: string;
        amount: number;
    }[];
}>();

const emit = defineEmits(['transactionDeleted']);

const deleteTransaction = (id: number) => {
    emit('transactionDeleted', id);
};
</script>

<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id"
            :class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{ transaction.text }} <span>KD {{ transaction.amount }}</span><button class="delete-btn"
                @click="deleteTransaction(transaction.id)">
                x
            </button>
        </li>
    </ul>
</template>
