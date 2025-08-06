<template>
    <v-form ref="form" @submit.prevent="submit" lazy-validation>
        <v-text-field
          v-model="product.name"
          label="Nombre"
          :rules="[v => !!v || 'El nombre es obligatorio']"
          required
        />
        <v-text-field 
        v-model.number="product.quantity"
        label="Cantidad"
        type="number"
        :rules="[v => v >= 0 || 'Cantidad no puede ser negativo']"
        />       
        <v-text-field 
        v-model.number="product.price"
        label="Precio"
        type="number"
        :rules="[v => v >= 0 || 'Precio no puede ser negativo']"
        required        
        />
        <v-text-field 
        v-model="product.expiryDate"
        label="Fecha de vencimiento"
        type="date"
        :rules="[v => !!v || 'Fecha es obligatorio']"
        required
        />

        <v-card-actions class="justify-end">
            <v-btn text @click="$emit('cancel')">Cancelar</v-btn>
            <v-btn color="primary" type="submit">Guardar</v-btn>
        </v-card-actions>
    </v-form>
</template>

<script setup>

import { ref, reactive, computed, watch, onMounted, toRefs } from 'vue'

const props = defineProps({
  initialProduct: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['save', 'cancel'])

const product = reactive({
  id: null,
  name: '',
  quantity: 0,
  price: 0,
  expiryDate: ''
})

watch(() => props.initialProduct, (newVal) => {
  Object.assign(product, newVal)
}, { immediate: true })

const form = ref(null)

function submit() {
  if (!product.id) {
    product.id = Date.now()
  }
  emit('save', { ...product }) 
}

</script>