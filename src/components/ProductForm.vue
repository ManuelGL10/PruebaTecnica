<template>
    <v-form ref="form" @submit.prevent="submit" lazy-validation>
        <v-text-field
          v-model="product.name"
          label="Nombre"
          :rules="[rules.required]"
          required
        />
        <v-text-field 
        v-model.number="product.quantity"
        label="Cantidad"
        type="number"
        :rules="[rules.required, rules.positive]"
        required
        />       
        <v-text-field 
        v-model.number="product.price"
        label="Precio"
        type="number"
        :rules="[rules.required, rules.positive]"
        required        
        />
        <v-text-field 
        v-model="product.expiryDate"
        label="Fecha de vencimiento"
        type="date"
        :rules="[rules.required, rules.futureDate]"
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
  quantity: null,
  price: null,
  expiryDate: ''
})

watch(() => props.initialProduct, (newVal) => {
  Object.assign(product, newVal)
}, { immediate: true })

const rules = {
  required:  v => (v !== null && v !== undefined && String(v).trim() !== '') || 'Este campo es obligatorio',
  positive: v => (!isNaN(v) && Number(v) > 0) || 'Debes ser un numero positivo', 
  futureDate: v => {
    if (!v) return 'Este campos es obligatorio'
    return new Date(v) > new Date() || 'La fecha debe ser futura'
  }
}

const form = ref(null)

async function submit() {
 const {valid} = await form.value.validate()
  if(!valid){
  return
 }
  if(!product.id){
    product.id = Date.now()
  }
  emit('save', {...product})
}

</script>