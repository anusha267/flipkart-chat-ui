<template>
    <section class="chat-list">
        <aside class="left-bar">
            <div class="header">
                <span class="filter-label bold">Filter by Title/OrderID</span>
                <input class="filter-input" placeholder="Start typing to search" v-model="filterValue" type="text" id="inputField">
            </div>
            <template v-for="chat in filteredChats" :key="chat.id">
                <div class="cursor-pointer" @click="chatClicked(chat)">
                    <ChatListCard :selected="selectedChat?.id === chat.id" :data="chat"></ChatListCard>
                </div>
            </template>
        </aside>
        <article >
            <ChatView v-if="selectedChat" :data="selectedChat" @messageAdded="onMessageAdded"/>
        </article>
    </section>
</template>
  
<script>
import { ref, watch, computed } from 'vue'
import ChatView from './ChatView.vue';
import ChatListCard from './ChatListCard.vue';
import { debounce } from '../utils/utils';

export default {
    components: {
        ChatView,
        ChatListCard,
    },
    props: {
        data: {
            type: Array,
            default: () => [],
        },
    },
    setup(props) {
        const filterValue = ref('');
        const chats = ref([]);
        const selectedChat = ref();

        const chatClicked = (chat) => {
            selectedChat.value = chat;
        };

        const debouncedSearch = debounce((value) => {
            filterValue.value = value;
        }, 300);

        watch(() => props.data, () => {
            chats.value = props.data;
        });
        const filteredChats = computed(() => {
            const filterText = filterValue.value.toLowerCase().trim();
            const filtered = chats.value.filter((chat) => {
                const title = chat.title.toLowerCase();
                const orderId = chat.orderId.toLowerCase();
                return title.includes(filterText) || orderId.includes(filterText);
            });

            return filtered.length > 0 ? filtered : chats.value;
        });

        const onMessageAdded = (msg) => {
            chats.value[msg.id -1]?.messageList.push(msg.msg);
            console.log(chats.value);
        };

        return {
            filterValue,
            chats,
            selectedChat,
            chatClicked,
            debouncedSearch,
            filteredChats,
            onMessageAdded,
        };
    },
};
</script>
  
  
<style>
.chat-list {
    display: flex;
    width: 100%;
}

.chat-list .left-bar {
    border-right: 1px solid #D4D4D4;
    width: 100%;
    height: calc(100vh - 80px);
    overflow: auto;
}
.header {
    display: flex;
    flex-direction: column;
    gap: 6px;
}
.filter-label{
    width: fit-content;
    font-size: 18px;
}
.filter-input {
    border: none;
    border-bottom:  2px solid #000000;
}
</style>