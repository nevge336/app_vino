<script setup>
import { ref } from 'vue';
import InputError from '@/Components/InputError.vue';
import TextInput from '@/Components/TextInput.vue';
import { useForm, usePage } from '@inertiajs/vue3';

// defineProps call removed

const cellar = usePage().props.cellar;

const form = useForm({
    name: cellar.name,
});


const editingName = ref(false);
const oldName = ref(cellar.name);

const startEditingName = () => {
    oldName.value = form.name;
    editingName.value = true;
};

const stopEditing = () => {
    form.name = oldName.value; //restaure l'ancien nom
    editingName.value = false;
};

const save = () => {
    form.patch(route('cellars.update', cellar.id), {
        preserveScroll: true,
        onSuccess: () => {
            oldName.value = form.name;
            stopEditing();
        },
    });
};
</script>

<template>
    <form @submit.prevent="save" class="profile">
        <h2>Mes celliers</h2>
        <div>
            <InputError :message="form.errors.name" />
            <div v-if="!editingName" class="__inline">
                <div class="flex-row">
                    <img src="/img/cellier-fond-blanc.png" class="icon">
                    <span>{{ form.name }}</span>
                </div>
                <img src="/img/icons/edit-3-black.svg" alt="Edit" class="icon-small" @click="startEditingName">
            </div>
            <div v-else>
                <div class="flex-row">
                    <img src="/img/cellier-fond-blanc.png" class="icon">

                    <TextInput
                    id="cellar_name"
                    type="text"
                    v-model="form.name"
                    autofocus
                    autocomplete="nom du cellier"
                    />
                    <img src="/img/icons/check-black.svg" alt="Accept" class="icon" @click="save">
                    <img src="/img/icons/x-black.svg" alt="Cancel" class="icon" @click="stopEditing">
                </div>
            </div>
        </div>
    </form>
</template>