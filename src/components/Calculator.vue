<script setup>
import { ref, onMounted } from 'vue';
const values = ref([
    {a: '', b: ''},
]);

let area = ref(0);
let totalClipboard = ref(0);
const clipBoard = ref({a: 5.95, b: 0.26});
const areaClipboard = ref(0);
const calculateResult = () => {
    areaClipboard.value = clipBoard.value.a * clipBoard.value.b;
    // calula area de cada item y sumarlo
    area.value = values.value.reduce((a, b) => a + b.a * b.b, 0);
    totalClipboard.value = area.value / areaClipboard.value;

    showData.value = !showData.value
}

const showData = ref(false)

const resetData = () => {
    values.value = [
        {a: '', b: ''},
    ];
    area.value = 0;
    totalClipboard.value = 0;
    showData.value = false
}

const type = ref('pvc')
const price1 = ref(0)
const price2 = ref(0)
const changeType = () => {
    if (type.value === 'pvc') {
        clipBoard.value.a = 5.95
        clipBoard.value.b = 0.26
    }
    if (type.value === 'ripado') {
        clipBoard.value.a = 2.95
        clipBoard.value.b = 0.17
    }
    if (localStorage.getItem('price1') && localStorage.getItem('price2')) {
        price1.value = localStorage.getItem('price1')
        price2.value = localStorage.getItem('price2')
    }
}

onMounted(() => {
    changeType()
})

</script>

<template>

    <div class="grid gap-3 grid-cols-2 p-2 mb-4 bg-white dark:bg-gray-900 antialiased ">
        <div class="flex items-center ps-4 border border-gray-200 rounded dark:border-gray-700">
            <input id="bordered-radio-1" v-model="type" @change="changeType" type="radio" value="pvc" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="bordered-radio-1" class="w-full py-4 ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Cielo falso</label>
        </div>
        <div class="flex items-center ps-4 border border-gray-200 rounded dark:border-gray-700">
            <input id="bordered-radio-2" v-model="type" @change="changeType" type="radio" value="ripado" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="bordered-radio-2" class="w-full py-4 ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Ripado</label>
        </div>
    </div>

    <form @submit.prevent="calculateResult">
        <div class="relative overflow-x-auto">
            <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                    <tr>
                        <th scope="col" class="px-6 py-3">
                            Lado A
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Lado B
                        </th>
                        <th scope="col" class="px-6 py-3">
                            
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700" v-for="(item, index) in values" :key="index">
                        <td class="px-2 py-3">
                            <input 
                                type="number"
                                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                placeholder="lado A"
                                required
                                min="0"
                                step="0.01"
                                v-model="item.a"
                            />
                        </td>
                        <td class="px-2 py-3">
                            <input
                                type="number"
                                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                placeholder="lado B"
                                required
                                min="0"
                                step="0.01"
                                v-model="item.b"
                            />
                        </td>
                        <td class="px-2 py-3">
                            <button
                                type="button"
                                class="text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-full text-sm p-2.5 text-center inline-flex items-center me-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800"
                                v-if="index > 0"
                                @click="values.splice(index, 1)"
                            >
                                <svg class="w-4 h-4 fill-current" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><title>close</title><path d="M19,6.41L17.59,5L12,10.59L6.41,5L5,6.41L10.59,12L5,17.59L6.41,19L12,13.41L17.59,19L19,17.59L13.41,12L19,6.41Z" /></svg>
                                <span class="sr-only">Icon description</span>
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
            
        </div>

        <div class="text-right pt-2">
            <button
                type="button"
                class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                @click="values.push({a: '', b: ''})"
            >Añadir</button>
        </div>

        <div class="pt-6">
            <button
                type="submit"
                class="w-full focus:outline-none text-white bg-green-700 hover:bg-green-800 focus:ring-4 focus:ring-green-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800"
            >Calcular</button>
        </div>
    </form>

    <!-- Main modal -->
    <div v-if="showData" id="authentication-modal" tabindex="-1" aria-modal="true" class="flex overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 justify-center items-center w-full md:inset-0 h-full max-h-full bg-black/50">
        <div class="relative p-4 w-full max-w-2xl max-h-full">
            <!-- Modal content -->
            <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
                <!-- Modal header -->
                <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600">
                    <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                        Resultado
                    </h3>
                    <button type="button" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white" data-modal-hide="default-modal" @click="showData = false">
                        <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"/>
                        </svg>
                        <span class="sr-only">Close modal</span>
                    </button>
                </div>
                <!-- Modal body -->
                <div class="p-4 md:p-5 space-y-4">
                    <div class="text-lg font-medium text-gray-900 dark:text-white">
                        Total de tablillas: {{ totalClipboard.toFixed(1) }}
                    </div>

                    <div v-if="type == 'pvc'" class="text-lg font-medium text-gray-900 dark:text-white">
                        Precio estimado: {{ (totalClipboard*price1).toFixed(2) }} /Bs.
                    </div>
                    <div v-else class="text-lg font-medium text-gray-900 dark:text-white">
                        Precio estimado: {{ (totalClipboard*price2).toFixed(2) }} /Bs.
                    </div>
                </div>
                <!-- Modal footer -->
                <div class="flex items-center justify-end p-4 md:p-5 border-t border-gray-200 rounded-b dark:border-gray-600">
                    <button type="button" class="focus:outline-none text-white bg-yellow-400 hover:bg-yellow-500 focus:ring-4 focus:ring-yellow-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:focus:ring-yellow-900" @click="showData = false">Editar datos</button>    
                    <button type="button" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800" @click="resetData()">Nuevo datos</button>    
                </div>
            </div>
        </div>
    </div>

</template>