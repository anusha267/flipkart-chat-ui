<template>
  <ChatList :data="data"/>
</template>

<script>
import ChatList from '@/components/ChatList.vue'
import { ref, onMounted } from 'vue';

export default {
  name: 'App',
  components: {
    ChatList
  },
  setup() {
    const data = ref([]);
    onMounted(()=>{
      // Make an API request when the component is mounted
      fetch('https://my-json-server.typicode.com/codebuds-fk/chat/chats') // Replace with your API endpoint
        .then((response) => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then((responseData) => {
          data.value = responseData;
          console.log(data.value);
        })
        .catch((error) => {
          console.error('Error fetching data:', error);
        });
    });
    return {
      data
    };
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
