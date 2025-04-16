<script setup>
import FormComponent from 'src/components/FormComponent.vue'
import TableComponent from 'src/components/TableComponent.vue'
import { getLocalStorage } from 'src/utils/utils'
import { ref } from 'vue'
const rows = ref(getLocalStorage('dataCompany'))
// Busca os dados do localStorage e armazena em rows
const updateTableData = () => {
  rows.value = getLocalStorage('dataCompany')
  // Atualiza os dados da tabela com os dados do localStorage
}
</script>
<template>
  <q-page padding>
    <FormComponent @data-updated="updateTableData" />
    <TableComponent :rows="rows" />
  </q-page>
</template>