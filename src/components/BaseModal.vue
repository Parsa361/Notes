<template>
    <div>
        <div v-if="showModal" class="overlay">
            <div class="modal">
                <textarea v-model="inputNote" name="note" id="note" cols="30" rows="10"></textarea>
                <p class="error-text" v-if="errorMessage">{{ errorMessage }}</p>
                <button @click="addNote">Add Note</button>
                <button @click="toggleModal" class="close">Close</button>
            </div>
        </div>

    </div>
</template>

<script setup>
import { ref } from 'vue';

const inputNote = ref('')
const errorMessage = ref('')


const props = defineProps({
    showModal: {
        type: Boolean,
        required: true
    },
})

const emit = defineEmits(
    ['add-note', 'toggle-modal']
)

const addNote = () => {
    if (!inputNote.value) {
        errorMessage.value = "Note can't be empty"
        return;
    }
    if (inputNote.value.length < 10) {
        errorMessage.value = "Note has to be more than 10 characters"
        return;
    }
    emit('add-note', inputNote.value);
    inputNote.value = ''
    errorMessage.value = ''
}
const toggleModal = () => {
    emit('toggle-modal');
    inputNote.value = ''
    errorMessage.value = ''
}
</script>

<style scoped>
.overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(95, 158, 160, 0.6);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: cenetr;
}

.modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
    margin: 0 auto;
}

.modal button {
    padding: 10px 20px;
    font-size: 1.25rem;
    width: 100%;
    background-color: blueviolet;
    border: none;
    cursor: pointer;
    margin-top: 15px;
    color: white;
}

.modal .close {
    background-color: rgb(193, 15, 15);
}

.error-text {
    font-size: 1rem;
    color: red;
    font-weight: normal;
    text-align: center;
}
</style>