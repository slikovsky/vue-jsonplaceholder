<script setup lang="ts" generic="T extends { id: number }">
const props = defineProps<{
  options: T[]
  label: string
}>()
const emit = defineEmits<{
  (e: 'update:modelValue', value: T | null): void
}>()

function onChange(event: Event) {
  const target = event.target as HTMLSelectElement
  const value = props.options.find((o) => o.id === Number(target.value)) || null

  emit('update:modelValue', value)
}
</script>

<template>
  <div>
    <label class="label">{{ label }}</label>
    <select class="select" @change="onChange">
      <option default value="">Выберите...</option>
      <option v-for="option in options" :key="option.id" :value="option.id">
        <slot :option="option" />
      </option>
    </select>
  </div>
</template>

<style scoped>
.select {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 10px;
  border: none;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  outline: none;
}

.select:hover {
  background: rgba(0, 0, 0, 0.9);
}

.label {
  margin-bottom: 6px;
  font-size: 20px;
}
</style>
