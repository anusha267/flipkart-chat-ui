<template>
    <div class="card" :class="{ 'bg-highlighted': selected}">
        <img :src="chat?.imageURL" width="30" height="30">
        <div class="card-details">
            <span class="card-detail text-12 bold">
                {{ chat?.title }}
            </span>
            <span v-if="chat?.orderId" class="card-detail text-12 bold">
                Order {{ chat?.orderId }}
            </span>
            <span class="card-detail text-12 color-secondary">
                {{ chat?.messageList[chat.messageList.length -1]?.message }}
            </span>
        </div>
        <div class="text-12 color-secondary">
            {{ formatTimestampToDateString(chat?.latestMessageTimestamp) }}
        </div>
    </div>
</template>
<script>
import { formatTimestampToDateString } from '../utils/date';

import { ref, watch } from 'vue'
export default {
    props: {
        selected: {
            type: Boolean,
            default: false,
        },
        data: {
            type: Object,
            default: () => {},
        },
    },
    setup(props) {
        const chat = ref(props.data);
        watch(() => props.data, () => {
            chat.value = props.data;
            console.log(chat);
        } );
        return {
            chat,
            formatTimestampToDateString,
        };
    }
}
</script>
<style scoped>
.card {
    display: flex;
    gap: 25px;
    border-bottom: 1px solid #D4D4D4;
    padding: 20px 30px;
}
.card-details {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
}
.card-detail {
    width: fit-content;
}
</style>