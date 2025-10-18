<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import { ref, onMounted } from 'vue';
import axios from 'axios';

// Create a reactive variable to store the list of translations.
// 'ref' makes it so that when this variable changes, the page will automatically update.
const translations = ref([]);

// Define a function to fetch translations from our API.
const getTranslations = async () => {
    try {
        // Make a GET request to the API endpoint.
        // The 'auth:sanctum' middleware handles authentication automatically.
        const response = await axios.get('/api/translations');
        // Update our 'translations' variable with the data from the API.
        translations.value = response.data.data;
    } catch (error) {
        console.error('Failed to fetch translations:', error);
    }
};

// 'onMounted' is a lifecycle hook that runs the code inside it
// once the Vue component has been fully loaded on the page.
onMounted(() => {
    getTranslations();
});
</script>

<template>
    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Translation History</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">

                        <div v-if="translations.length === 0">
                            You don't have any saved translations yet. Use Postman to add some!
                        </div>

                        <ul v-else class="space-y-4">
                            <li v-for="translation in translations" :key="translation.id" class="border-b pb-2">
                                <p class="text-lg font-medium">{{ translation.translated_text }}</p>
                                <span class="text-sm text-gray-500">
                                    Received on: {{ new Date(translation.created_at).toLocaleString() }}
                                </span>
                            </li>
                        </ul>

                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
