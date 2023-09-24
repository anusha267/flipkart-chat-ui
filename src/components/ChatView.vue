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
            <div v-for="msg in chat?.messageList" :key="msg" class="msg-wrapper">
                <BotChat v-if="msg.sender === userType.BOT" :data="msg"/>
                <UserChat v-if="msg.sender === userType.USER" :data="msg"/>
            </div>
        </div>
        <div class="msg-input">
            <input type="text" v-model="chatInput" placeholder="Type a Message">
            <button class="bg-flipkart-blue" @click="sendClicked">Send</button>
        </div>
    </section>
</template>
<script>
import BotChat from './BotChat.vue';
import UserChat from './UserChat.vue';
import { ref, watch } from 'vue'
import {userType} from '../constants/constants';
export default {
    components: {
        BotChat,
        UserChat,
    },
    emits:['messageAdded'],
    props: {
        data: {
            type: Object,
            default: () => {},
        },
    },
    setup(props, {emit}) {
        const chatInput = ref('');
        const chat = ref(props.data);
        const sendClicked = () => {
            console.log(chatInput.value);
            emit('messageAdded', {
                msg: {
                 message: chatInput.value,
                 timestamp: new Date().getTime(),
                 msgId: `msg${chat.value?.messageList.length}`,
                 type: '',
                 sender: userType.USER,
                },
                id: chat.value?.id,
            });
            chatInput.value = '';
        }
        watch(() => props.data, () => {
            chat.value = props.data;
        } );
        return {
            chat,
            userType,
            chatInput,
            sendClicked,
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
    overflow: auto;
}
.empty-msg {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}
.msg-wrapper {
    padding: 15px 20px;
}
.msg-input {
    display: flex;
}
.msg-input input {
    flex-grow: 1;
    border: none;
    height: 40px;
}
.msg-input button {
    border: none;
    color: white;
    padding: 5px 15px;
    border-radius: 8px;
    cursor: pointer;
}
</style>