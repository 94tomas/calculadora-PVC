<script setup>
    import { ref } from 'vue'
    import Calculator from './Calculator.vue'
    import Settings from './Settings.vue'

    const showCalculator = ref(true)
    const changeData = () => {
        showCalculator.value = false
        setTimeout(() => {
            showCalculator.value = true
        }, 300);
    }
</script>

<template>
    <header>			
        <nav class="bg-white border-gray-200 dark:bg-gray-900">
            <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
                <a href="https://flowbite.com/" class="flex items-center space-x-3 rtl:space-x-reverse">
                    <!-- <img src="https://flowbite.com/docs/images/logo.svg" class="h-8" alt="Flowbite Logo" /> -->
                    <span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white">Calculadora</span>
                </a>

                <Settings @change-data="changeData" client:load />
            </div>
        </nav>

    </header>
    
    <main>
        <!-- container -->
        <div class="w-full max-w-lg mx-auto p-4">

            <Calculator v-if="showCalculator" client:load />
            
            <div v-else role="status" class="max-w-sm animate-pulse">
                <div class="h-2.5 bg-gray-200 rounded-full dark:bg-gray-700 w-48 mb-4"></div>
                <div class="h-2 bg-gray-200 rounded-full dark:bg-gray-700 max-w-[360px] mb-2.5"></div>
                <div class="h-2 bg-gray-200 rounded-full dark:bg-gray-700 mb-2.5"></div>
                <div class="h-2 bg-gray-200 rounded-full dark:bg-gray-700 max-w-[330px] mb-2.5"></div>
                <div class="h-2 bg-gray-200 rounded-full dark:bg-gray-700 max-w-[300px] mb-2.5"></div>
                <div class="h-2 bg-gray-200 rounded-full dark:bg-gray-700 max-w-[360px]"></div>
                <span class="sr-only">Loading...</span>
            </div>

        </div>
    </main>
</template>