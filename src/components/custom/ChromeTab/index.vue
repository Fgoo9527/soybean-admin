<template>
  <div
    class="relative flex-y-center h-34px px-24px cursor-pointer"
    :class="{ '-mr-18px': !isLast, 'z-10': isActive, 'z-9': isHover }"
    @mouseenter="setTrue"
    @mouseleave="setFalse"
  >
    <div class="absolute-lb wh-full overflow-hidden">
      <svg-radius-bg
        class="wh-full"
        :is-active="isActive"
        :is-hover="isHover"
        :dark-mode="darkMode"
        :primary-color="primaryColor"
      />
    </div>
    <span class="relative whitespace-nowrap z-2">
      <slot></slot>
    </span>
    <div v-if="closable" class="pl-18px">
      <icon-close :is-primary="isActive" :primary-color="primaryColor" @click="handleClose" />
    </div>
    <n-divider v-if="!isHover && !isActive" :vertical="true" class="absolute right-0 !bg-[#a4abb8] z-2" />
  </div>
</template>

<script setup lang="ts">
import { NDivider } from 'naive-ui';
import { useBoolean } from '@/hooks';
import IconClose from '../IconClose/index.vue';
import { SvgRadiusBg } from './components';

interface Props {
  /** 激活状态 */
  isActive?: boolean;
  /** 主题颜色 */
  primaryColor?: string;
  /** 是否显示关闭图标 */
  closable?: boolean;
  /** 暗黑模式 */
  darkMode?: boolean;
  /** 是否是最后一个 */
  isLast: boolean;
}

withDefaults(defineProps<Props>(), {
  isActive: false,
  primaryColor: '#409EFF',
  closable: true,
  darkMode: false,
  isLast: false
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
</script>
<style scoped></style>
