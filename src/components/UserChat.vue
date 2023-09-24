<template>
    <div class="wrapper">
        <div class="bg-flipkart-blue user-chat text-12">
            <span class="color-white">{{ msg?.message }}</span>
            <div class="time text-10 color-white">
                <span>{{ timestampToTime(msg?.timestamp) }}</span>
            </div>
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
.user-chat {
    display: flex;
    width: fit-content;
    flex-direction: column;
    border-radius: 8px;
    padding: 10px 10px;
    border-top-right-radius: 0;
}
.user-chat .time {
    display: flex;
    justify-content: end;
}
.wrapper {
    display: flex;
    flex-direction: row-reverse;
}
</style>