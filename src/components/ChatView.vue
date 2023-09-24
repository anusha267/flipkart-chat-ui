<template>
    <section class="chat-view">
        <header class="heading">
            <img :src="chat?.imageURL" width="30" height="30">
            <span class="bold">
                {{ chat?.title }}
            </span>
        </header>
        <div class="chat-wrapper bg-highlighted">
            <div class="empty-msg bold" v-if="!chat?.messageList || chat?.messageList.length === 0">
                Send a message to start chatting
            </div>
            <div v-for="msg in chat?.messageList" :key="msg">
                {{ msg.message }}
            </div>
        </div>
        
    </section>
</template>
<script>
import { ref, watch } from 'vue'
export default {
    props: {
        data: {
            type: Object,
            default: () => {},
        },
    },
    setup(props) {
        const chat = ref(props.data);
        watch(() => props.data, () => {
            chat.value = props.data;
            console.log(chat.value.title);
        } );
        return {
            chat,
        };
    }
}
</script>
<style scoped>
.chat-view {
    width: 60vw;
}
.heading {
    display: flex;
    gap: 40px;
    padding: 20px 40px;
    align-items: center;
    border-bottom: 1px solid #D4D4D4;;
}
.img {
    height: 300px;
    width: 100%;
    object-fit: cover;
}
.chat-wrapper {
    height: calc(100vh - 200px);
}
.empty-msg {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>