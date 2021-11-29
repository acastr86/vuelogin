<script setup>
import { onUnmounted, ref } from 'vue';
import useChat from '../composable/useChat';
import useAuth from '../composable/useAuth';

import UndrawChatting from "vue-undraw/UndrawChatting.vue";

const {messages, unsubscribe, sendMessage}= useChat();
const {user} = useAuth();

const newMessage = ref('')

const send = () => {
    sendMessage(newMessage.value)
    newMessage.value = ''
};

onUnmounted(() => {
    unsubscribe()
});

</script>

<template>

<div class="flex flex-col items-center justify-center space-y-6 min-h-screen-nonav">
    <div class="flex items-center justify-center overflow-hidden bg-blue-100 rounded-lg shadow-2xl">
       
        <div>
            <h1 class="mt-8 text-6xl font-thin tracking-tighter text-center">
                Cool Chat
           </h1>
            <UndrawChatting class="h-64" />
        </div>
        
       
        <div class="min-h-[500px] w-full mt-8 rounded-lg shadow-2xl flex flex-col justify-between bg-white min-w-[300px]">
            <ul class="p-4 space-y-4">
                <li v-for="message in messages" :key="message.id">
                    <div class="flex justify-between px-4 py-2 rounded-lg"
                        :class="user === message.author ? 'bg-yellow-200' : 'bg-gray-200'">
                        <span>{{ message.text }}</span><span>by {{ message.author }}</span>
                    </div>
                </li>
            </ul>
            <div>
                <input class="w-full p-4 rounded-lg focus:outline-none focus:bg-yellow-200"
                    type="text" placeholder="Type a message..." v-model="newMessage"
                    @change="send" />
                </div>
            </div>


    </div>
</div>

    
</template>