<template>
    <div class="q-pa-md bg-grey-4">
        <q-form @submit="onSubmit" @reset="onReset" class="q-col-gutter-x-md">
            <div class="row q-col-gutter-md">
                <q-input filled bg-color="white" v-model="name" label="Nome *" class="col-6" />
                <q-input filled bg-color="white" v-model="description" label="Descrição *" class="col-6" />
                <q-input filled bg-color="white" v-model="telephone" label="Telefone *" class="col-6" />
                <q-input filled bg-color="white" v-model="address" label="Endereço *" class="col-6" />
            </div>
            <div class="q-mt-md">
                <q-btn label="Submit" type="submit" color="primary" />
                <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
            </div>
        </q-form>
    </div>
</template>

<script setup>
import { addNewData } from 'src/utils/utils'
import { ref, defineEmits } from 'vue'

const emit = defineEmits(['dataUpdated'])
const name = ref('')
const description = ref('')
const telephone = ref('')
const address = ref('')

const onSubmit = () => {
    const data = {
        name: name.value,
        description: description.value,
        telephone: telephone.value,
        address: address.value,
    }
    addNewData('dataCompany', data)
    emit('dataUpdated')
    onReset()
}

const onReset = () => {
    name.value = ''
    description.value = ''
    telephone.value = ''
    address.value = ''
}
</script>