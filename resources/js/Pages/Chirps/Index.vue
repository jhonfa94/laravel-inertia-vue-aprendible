<script setup>
import { ref } from 'vue';
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import InputError from '@/Components/InputError.vue';

// const title = 'Title from Vue'

const message = ref('');
const errors = ref({});

const submit = () => {
    console.log("message: ", message.value)
    axios.post('/chirps', { message: message.value })
        .then((res) => {
            console.log(res.data);

            message.value = ''
            errors.value = {}
        })
        .catch((error) => {
            // 422 => Error de validación
            if (error.response.status === 422) {
                errors.value = error.response.data.errors;
            }
            console.error(error.response.data.message);

        })
}

defineProps(['title', 'subtitle'])

</script>

<template>
    <Head title="Chirps" />
    <AuthenticatedLayout>

        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">{{ title }}</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-sm sm:rounded-lg">
                    <!-- <div class="p-6 text-gray-900 dark:text-gray-100"> {{ subtitle }}</div> -->
                    <form
                        @submit.prevent="submit"
                    >
                        <textarea
                        v-model="message"
                        placeholder="What's on your mind?"
                            class="block w-full rounded-md border-gray-300 bg-white"

                        ></textarea>
                        <!-- <div v-if="errors.message"
                            class="text-red-500"                        >
                            {{ errors.message[0] }}
                        </div> -->
                        <InputError :message="errors.message && errors.message[0]"/>
                        <PrimaryButton
                            class="mt-2"
                        >
                            Chirp
                        </PrimaryButton>


                    </form>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>


</template>

