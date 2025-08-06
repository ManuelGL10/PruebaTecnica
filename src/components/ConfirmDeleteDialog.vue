<template>
    <v-dialog :model-value="modelValue" @update:modelValue="$emit('update:modelValue', $event)" max-width="500px">
      <v-card>
        <v-card-title class="text-h6">Confirmar Eliminación</v-card-title>
        <v-card-text>
          ¿Estás seguro de que deseas eliminar el producto
          <strong>{{ item.name }}</strong> con fecha de vencimiento
          <strong>{{ item.expiryDate }}</strong>?
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="grey" text @click="$emit('update:modelValue', false)">Cancelar</v-btn>
          <v-btn color="red" text @click="confirmDelete">Eliminar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </template>

<script setup>
  const props = defineProps({
  modelValue: Boolean,
  item: Object,
})

const emit = defineEmits(['update:modelValue', 'confirm'])

const confirmDelete = () => {
  emit('confirm')
  emit('update:modelValue', false)
}

</script>
  