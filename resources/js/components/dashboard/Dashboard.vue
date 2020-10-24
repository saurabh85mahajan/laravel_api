<template>
    <div class="p-4 m-4 bg-white rounded flex">
        <div>
            <h1 class="text-2xl text-gray-700">Dashboard</h1>

            <p class="mt-4">
                Hello <span class="font-bold">{{ user.name}}</span>, Welcome to the Dashboard. You have registed with the email <span class="italic">{{ user.email}}</span>
            </p>

            <p class="mt-2">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
            <p class="mt-2">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>               

        </div>
    </div>
</template>

<script>
import {mapGetters} from 'vuex';

export default {
    computed: {
        ...mapGetters([
            'user'
        ])    
    }
}
</script>