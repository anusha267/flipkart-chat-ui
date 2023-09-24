<template>
    <nav>
        <span>Filter by Title/OrderID</span>
        <input v-model="filterValue" type="text" id="inputField">
    </nav>
    <section class="chat-list">
        <aside class="left-bar">
            <template v-for="chat in filteredChats" :key="chat.id">
                <div class="cursor-pointer" @click="chatClicked(chat)">
                    <ChatListCard :selected="selectedChat?.id === chat.id" :data="chat"></ChatListCard>
                </div>
            </template>
        </aside>
        <article >
            <ChatView v-if="selectedChat" :data="selectedChat"/>
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

        // Create a computed property to filter chats based on filterValue
        const filteredChats = computed(() => {
            const filterText = filterValue.value.toLowerCase().trim();
            const filtered = chats.value.filter((chat) => {
                const title = chat.title.toLowerCase();
                const orderId = chat.orderId.toLowerCase();
                return title.includes(filterText) || orderId.includes(filterText);
            });

            // If no results found, return all chats
            return filtered.length > 0 ? filtered : chats.value;
        });

        return {
            filterValue,
            chats,
            selectedChat,
            chatClicked,
            debouncedSearch,
            filteredChats,
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
    border: 1px solid #D4D4D4;
    width: 100%;
}
</style>