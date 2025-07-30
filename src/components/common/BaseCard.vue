<template>
  <div :class="cardClasses">
    <!-- Header -->
    <div v-if="$slots.header || title" :class="headerClasses">
      <slot name="header">
        <h3 v-if="title" class="text-lg font-semibold text-gray-900">
          {{ title }}
        </h3>
      </slot>
    </div>

    <!-- Body -->
    <div :class="bodyClasses">
      <slot />
    </div>

    <!-- Footer -->
    <div v-if="$slots.footer" :class="footerClasses">
      <slot name="footer" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";

interface Props {
  title?: string;
  variant?: "default" | "elevated" | "outlined";
  padding?: "none" | "sm" | "md" | "lg";
  shadow?: "none" | "sm" | "md" | "lg";
  hover?: boolean;
  clickable?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "default",
  padding: "md",
  shadow: "md",
  hover: false,
  clickable: false,
});

const emit = defineEmits<{
  click: [event: MouseEvent];
}>();

const cardClasses = computed(() => {
  const baseClasses = "bg-white rounded-lg border";

  const variantClasses = {
    default: "border-gray-200",
    elevated: "border-gray-200",
    outlined: "border-gray-300",
  };

  const shadowClasses = {
    none: "",
    sm: "shadow-sm",
    md: "shadow-md",
    lg: "shadow-lg",
  };

  const hoverClasses = props.hover
    ? "hover:shadow-lg transition-shadow duration-200"
    : "";
  const clickableClasses = props.clickable ? "cursor-pointer" : "";

  return `${baseClasses} ${variantClasses[props.variant]} ${
    shadowClasses[props.shadow]
  } ${hoverClasses} ${clickableClasses}`;
});

const headerClasses = computed(() => {
  const baseClasses = "px-6 py-4 border-b border-gray-200";

  const paddingClasses = {
    none: "px-0 py-0",
    sm: "px-4 py-3",
    md: "px-6 py-4",
    lg: "px-8 py-6",
  };

  return `${baseClasses} ${paddingClasses[props.padding]}`;
});

const bodyClasses = computed(() => {
  const paddingClasses = {
    none: "p-0",
    sm: "p-4",
    md: "p-6",
    lg: "p-8",
  };

  return paddingClasses[props.padding];
});

const footerClasses = computed(() => {
  const baseClasses = "px-6 py-4 border-t border-gray-200 bg-gray-50";

  const paddingClasses = {
    none: "px-0 py-0",
    sm: "px-4 py-3",
    md: "px-6 py-4",
    lg: "px-8 py-6",
  };

  return `${baseClasses} ${paddingClasses[props.padding]}`;
});
</script>
