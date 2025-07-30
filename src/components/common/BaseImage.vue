<template>
  <div :class="containerClasses">
    <img
      :src="src"
      :alt="alt"
      :class="imageClasses"
      :loading="loading"
      :decoding="decoding"
      @load="handleLoad"
      @error="handleError"
      @click="handleClick" />

    <!-- Loading skeleton -->
    <div v-if="loading && !imageLoaded" :class="skeletonClasses">
      <div class="animate-pulse bg-gray-200 rounded w-full h-full"></div>
    </div>

    <!-- Error fallback -->
    <div v-if="hasError" :class="fallbackClasses">
      <div
        class="flex items-center justify-center w-full h-full bg-gray-100 rounded">
        <svg
          class="w-8 h-8 text-gray-400"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24">
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
        </svg>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

interface Props {
  src: string;
  alt: string;
  width?: string | number;
  height?: string | number;
  fit?: "cover" | "contain" | "fill" | "none" | "scale-down";
  radius?: "none" | "sm" | "md" | "lg" | "full";
  loading?: "lazy" | "eager";
  decoding?: "async" | "sync" | "auto";
  clickable?: boolean;
  hover?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  fit: "cover",
  radius: "none",
  loading: "lazy",
  decoding: "async",
  clickable: false,
  hover: false,
});

const emit = defineEmits<{
  load: [event: Event];
  error: [event: Event];
  click: [event: MouseEvent];
}>();

const imageLoaded = ref(false);
const hasError = ref(false);

const containerClasses = computed(() => {
  const baseClasses = "relative overflow-hidden";
  const clickableClasses = props.clickable ? "cursor-pointer" : "";
  return `${baseClasses} ${clickableClasses}`;
});

const imageClasses = computed(() => {
  const baseClasses = "w-full h-full transition-transform duration-200";

  const fitClasses = {
    cover: "object-cover",
    contain: "object-contain",
    fill: "object-fill",
    none: "object-none",
    "scale-down": "object-scale-down",
  };

  const radiusClasses = {
    none: "",
    sm: "rounded-sm",
    md: "rounded-md",
    lg: "rounded-lg",
    full: "rounded-full",
  };

  const hoverClasses = props.hover ? "hover:scale-105" : "";

  return `${baseClasses} ${fitClasses[props.fit]} ${
    radiusClasses[props.radius]
  } ${hoverClasses}`;
});

const skeletonClasses = computed(() => {
  return "absolute inset-0 bg-gray-200 animate-pulse";
});

const fallbackClasses = computed(() => {
  const baseClasses = "absolute inset-0";
  const radiusClasses = {
    none: "",
    sm: "rounded-sm",
    md: "rounded-md",
    lg: "rounded-lg",
    full: "rounded-full",
  };
  return `${baseClasses} ${radiusClasses[props.radius]}`;
});

const handleLoad = (event: Event) => {
  imageLoaded.value = true;
  hasError.value = false;
  emit("load", event);
};

const handleError = (event: Event) => {
  hasError.value = true;
  imageLoaded.value = false;
  emit("error", event);
};

const handleClick = (event: MouseEvent) => {
  if (props.clickable) {
    emit("click", event);
  }
};
</script>
