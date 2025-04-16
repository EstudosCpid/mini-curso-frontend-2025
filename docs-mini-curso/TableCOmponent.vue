<template>
    <div class="q-pa-md bg-grey-4 q-mt-md">
        <q-table title="Visualização de dados" :rows="rowsData" :columns="columns" row-key="id">
            <template v-slot:body="props">
                <q-tr :props="props">
                    <q-td v-for="col in columns.slice(0, -1)" :key="col.name" :props="props">
                        <div title="Clique para editar" v-if="col.name !== 'id'">
                            {{ props.row[col.field] }}
                        </div>
                        <q-popup-edit v-model="props.row[col.field]" v-if="col.name !== 'id'" v-slot="scope">
                            <q-input v-model="scope.value" dense autofocus counter @keyup.enter="editar(
                                props.row.id,
                                col.field,
                                scope.value,
                                scope
                            )" />
                        </q-popup-edit>
                        <div v-else>
                            {{ props.row[col.field] }}
                        </div>
                    </q-td>
                    <q-td :key="'delete'" :props="props">
                        <q-btn color="negative" label="Excluir" @click="handleDelete('dataCompany', props.row.id)" />
                    </q-td>
                </q-tr>
            </template>
        </q-table>
    </div>
</template>

<script setup lang="js">
import { ref, defineProps, watch } from 'vue'
import { getLocalStorage, deleteData } from 'src/utils/utils.js'
const props = defineProps({
    rows: {
        type: Array,
        required: true,
    },
})
const columns = ref([
    {name: 'id', label: 'ID', align: 'left', field: 'id', sortable: true },    
    {name: 'name', label: 'Nome', align: 'left', field: 'name', sortable: true},
    {name: 'description', label: 'Descrição', align: 'left', field:'description', sortable: true},
    {name: 'telephone', label: 'Telefone', align: 'left', field:'telephone', sortable: true},
    {name: 'address', label: 'Endereço', align: 'left', field: 'address', sortable: true},
    {name: 'delete', label: 'Excluir', align: 'center', field: 'delete' },
])

const rowsData = ref(props.rows)
watch(
    () => props.rows,
    (newVal) => {
        rowsData.value = newVal
    },
)

const editar = (id, field, value, scope) => {
    const index = rowsData.value.findIndex((row) => row.id === id)
    rowsData.value[index][field] = value
    localStorage.setItem('dataCompany', JSON.stringify(rowsData.value))
    scope.set()
}

const handleDelete = (key, id) => {
    deleteData(key, id)
    rowsData.value = getLocalStorage('dataCompany')
}
</script>