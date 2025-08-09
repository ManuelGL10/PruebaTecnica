<template>
    <v-container>
      <v-card>
        <v-row class="align-center mb-4 mt-4 ml-4 mr-4">
        <v-col cols="6">
            <h2 class="text-h5">Listado de Productos</h2>
        </v-col>
        <v-col cols="6" class="text-right">
            <v-btn color="primary" @click="openCreateForm">
            Agregar Producto
            </v-btn>
        </v-col>
        </v-row>
        <v-data-table
          :headers="headers"
          :items="products"
          class="elevation-1"
        >
          <template v-slot:item.expiryDate="{ item }">
            <div
              :class="{
                'bg-red-lighten-3': getExpiryStatus(item.expiryDate) === 'expired',
                'bg-yellow-lighten-3': getExpiryStatus(item.expiryDate) === 'warning',
                'bg-green-lighten-3': getExpiryStatus(item.expiryDate) === 'good'
              }"
              class="pa-2 rounded"
              style="display: inline-block; min-width: 120px; text-align: center; color: black;"
            >
              {{ formatDate(item.expiryDate) }}
            </div>
          </template>
  
          <template v-slot:item.actions="{ item, index }">
            <v-icon
              small
              class="mr-2"
              color="primary"
              @click="editProduct(index)"
              style="cursor: pointer;"
              title="Editar"
            >
              mdi-pencil
            </v-icon>
            <v-icon
              small
              color="red"
              @click="confirmDelete(item)"
              style="cursor: pointer;"
              title="Eliminar"
            >
              mdi-delete
            </v-icon>
          </template>
          <template v-slot:no-data>
                <v-alert type="info" color=" lighten-4">
                No se encontró información de productos.
                </v-alert>
           </template>
        </v-data-table>
      </v-card>
        <v-dialog v-model="showForm" persistent max-width="500px">
            <v-card>
             <v-card-title class="text-h6">
                {{ editingIndex === null ? 'Agregar Producto' : 'Editar Producto' }}
             </v-card-title>
             <v-card-text>
                <ProductForm
                    :initialProduct="{ ...editingProduct }"
                    @save="handleSave"
                    @cancel="closeForm"
                />
             </v-card-text>
            </v-card>
        </v-dialog>
        <ConfirmDeleteDialog
            v-if="selectedItem"
            v-model="showDialog"
            :item="selectedItem"
            @confirm="deleteProduct"
        />
    </v-container>
  </template>
  
<script setup>
  import { ref, onMounted } from 'vue'
  import ProductForm from './ProductForm.vue'
  import ConfirmDeleteDialog from './ConfirmDeleteDialog.vue'

const headers = [
  { title: 'Nombre', value: 'name', align: 'center' },
  { title: 'Cantidad', value: 'quantity', align: 'center' },
  { title: 'Precio', value: 'price', align: 'center' },
  { title: 'Fecha de Vencimiento', value: 'expiryDate', align: 'center' },
  { title: 'Acciones', value: 'actions', align: 'center', sortable: false }
]

const products = ref([])
const showForm = ref(false)
const editingProduct = ref(getEmptyProduct())
const editingIndex = ref(null)

onMounted(() => {
  const stored = localStorage.getItem('products')
  products.value = stored ? JSON.parse(stored) : []
})


const formatDate = (dateStr) => {
 return new Date(dateStr).toDateString()
}

const getExpiryStatus = (expiryDate) => {
  const today = new Date()
  today.setDate(0,0,0,0)
  const expiry = new Date(expiryDate)
  expiry.setHours(0,0,0,0)

  const diffDay = Math.floor((expiry - today) / (1000 * 60 * 60 * 24))
  if (diffDay < 0) return 'expired'
  if (diffDay <= 7) return 'warning'
  return 'good'
}

function getEmptyProduct(){
  return{
    id:null,
    name: '',
    quantity: 0,
    price: 0,
    expiryDate:''
  }
}

function openCreateForm(){
  editingProduct.value = getEmptyProduct()
  editingIndex.value = null
  showForm.value = true
}

function handleSave(product) {
  if (editingIndex.value !== null) {
    products.value[editingIndex.value] = product
  } else {
    products.value.push(product)
  }
  localStorage.setItem('products', JSON.stringify(products.value))
  editingProduct.value = getEmptyProduct()
  editingIndex.value = null
  showForm.value = false
}

function closeForm() {
  showForm.value = false
}


const showDialog = ref(false)
const selectedItem = ref(null)

function confirmDelete(item) {
  selectedItem.value = item
  showDialog.value = true
}

function deleteProduct() {
  products.value = products.value.filter(p => p.id !== selectedItem.value.id)
  localStorage.setItem('products', JSON.stringify(products.value))
  showDialog.value = false
  selectedItem.value = null
}

</script>
  