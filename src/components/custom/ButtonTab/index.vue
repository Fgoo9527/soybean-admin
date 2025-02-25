<template>
  <div
    class="relative flex-center h-30px pl-14px border-1px border-[#e5e7eb] dark:border-[#ffffff3d] rounded-2px transition-border-color duration-300 ease-in-out cursor-pointer"
    :class="[closable ? 'pr-6px' : 'pr-14px']"
    :style="buttonStyle"
    @mouseenter="setTrue"
    @mouseleave="setFalse"
  >
    <span class="whitespace-nowrap">
      <slot></slot>
    </span>
    <div v-if="closable" class="pl-10px">
      <icon-close :is-primary="isActive || isHover" :primary-color="primaryColor" @click="handleClose" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { computed } from 'vue';
import { useBoolean } from '@/hooks';
import { IconClose } from '@/components';
import { addColorAlpha } from '@/utils';

interface Props {
  /** 激活状态 */
  isActive?: boolean;
  /** 主题颜色 */
  primaryColor?: string;
  /** 是否显示关闭图标 */
  closable?: boolean;
  /** 暗黑模式 */
  darkMode?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  isActive: false,
  primaryColor: '#409EFF',
  closable: true,
  darkMode: false
});
const emit = defineEmits<{
  /** 点击关闭图标 */
  (e: 'close'): void;
}>();

const { bool: isHover, setTrue, setFalse } = useBoolean();

function handleClose(e: MouseEvent) {
  e.stopPropagation();
  emit('close');
}

const buttonStyle = computed(() => {
  const style: { [key: string]: string } = {};
  if (props.isActive || isHover.value) {
    style.color = props.primaryColor;
    style.borderColor = addColorAlpha(props.primaryColor, 0.3);
    if (props.isActive) {
      const alpha = props.darkMode ? 0.15 : 0.1;
      style.backgroundColor = addColorAlpha(props.primaryColor, alpha);
    }
  }
  return style;
});
</script>
<style scoped></style>
