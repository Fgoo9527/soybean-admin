<template>
  <div>
    <canvas ref="domRef" width="152" height="40" class="cursor-pointer" @click="getImgCode"></canvas>
  </div>
</template>

<script setup lang="ts">
import { watch } from 'vue';
import { useImageVerify } from '@/hooks';

interface Props {
  code: string;
}

interface Emits {
  (e: 'update:code', code: string): void;
}

const props = defineProps<Props>();

const emit = defineEmits<Emits>();

const { domRef, imgCode, setImgCode, getImgCode } = useImageVerify();

watch(
  () => props.code,
  newValue => {
    setImgCode(newValue);
  }
);
watch(imgCode, newValue => {
  emit('update:code', newValue);
});

defineExpose({ getImgCode });
</script>
<style scoped></style>
