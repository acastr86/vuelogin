<script setup>
import {ref} from "vue";
import {useRouter} from "vue-router";
import UndrawLogin from "vue-undraw/UndrawLogin.vue";
import useAuth from "../composable/useAuth";
import useError from "../composable/useError";

const {isAuthenticated, login, signup} = useAuth();

const username = ref("");
const password = ref("");

const router = useRouter();

const logginIn = async () => {
    await login(username.value,  password.value);
    goToHome();
};

const signingUp = async () => {
    await signup(username.value,  password.value);
    goToHome();
};

const goToHome = () => {
    if(isAuthenticated.value) {
        router.push("/");
    } else {
        setError("Invalid username or password");
        start();
    }
};

const {error, setError} = useError();

import {useTimeout, promiseTimeout} from "@vueuse/core"

const {ready, start} = useTimeout(3000, { controls: true });

</script>

<template>
<div class="flex flex-col items-center justify-center space-y-12 min-h-screen-nonav">
    <div class="flex items-center justify-center overflow-hidden bg-blue-100 rounded-lg shadow-2xl">
        
        <UndrawLogin class="h-64 bg-white" />
        <form @submit.prevent="logginIn" class="flex flex-col p-4 space-y-4">
            <input type="text" class="p-2 border-2 rounded-lg"
              placeholder="Username" v-model="username" />
              <input type="password" class="p-2 border-2 rounded-lg"
              placeholder="Password" v-model="password" />
              <div class="flex space-x-2">
                  <button @submit.prevent="logginIn" type="submit" class="w-1/2 py-2 text-blue-200 bg-blue-600 rounded-lg">Login</button>
                  <button @click="signingUp" class="w-1/2 py-2 text-yellow-200 bg-yellow-600 rounded-lg">Sign Up</button>
              </div>
              
        </form>
    </div>
    <div v-if="!ready && error" class="absolute w-1/3 p-4 text-center text-red-800 transition-opacity duration-1000 bg-red-300 rounded-lg bottom-2 right-2"
    :class="!ready && error ? 'opacity-100' : 'opacity-0' ">
        {{error}}  
    </div>
</div>
    
</template>