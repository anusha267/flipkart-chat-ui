<template>
    <div class="bot-chat text-12">
        <span class="std-msg">{{ msg?.message }}</span>
        <div class="time std-msg text-10 color-secondary">
            <span>{{ timestampToTime(msg?.timestamp) }}</span>
        </div>
        <div v-if="msg?.messageType === 'optionedMessage'" class="options-wrapper bg-highlighted">
            <button class="option cursor-pointer bold color-flipkart-blue" v-for="option in msg?.options" :key="option">
                {{ option.optionText }}
            </button>
        </div>
    </div>
</template>
<script>
import { ref, watch } from 'vue';
import { timestampToTime } from '../utils/date';
export default {
    props: {
        data: {
            type: Object,
            default: () => {},
        },
    },
    setup(props) {
        const msg = ref(props.data);
        watch(() => props.data, () => {
            msg.value = props.data;
        } );
        return {
            msg,
            timestampToTime,
        };
    }
}
</script>
<style scoped>
.bot-chat {
    display: flex;
    width: fit-content;
    flex-direction: column;
    background: white;
    border-radius: 8px;
    border: 1px solid rgb(223, 223, 223);
    border-top-left-radius: 0;
}
.std-msg {
    padding: 8px 10px;
}
.bot-chat .time {
    padding: 2px 10px;
    display: flex;
    justify-content: end;
}
.options-wrapper {
    display: flex;
    flex-direction: column;
    border-bottom-left-radius: 8px;
    border-bottom-right-radius: 8px;
}
.options-wrapper .option {
    border: none;
    padding: 10px 30px;
    border-bottom: 1px solid rgb(223, 223, 223);
}
</style>