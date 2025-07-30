<template>
  <span :class="badgeClasses">
    <slot />
  </span>
</template>

<script setup lang="ts">
import { computed } from "vue";

interface Props {
  variant?: "primary" | "secondary" | "success" | "warning" | "danger";
  size?: "sm" | "md" | "lg";
  rounded?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: "primary",
  size: "md",
  rounded: false,
});

const badgeClasses = computed(() => {
  const baseClasses = "inline-flex items-center font-medium";

  const variantClasses = {
    primary: "bg-blue-100 text-blue-800",
    secondary: "bg-gray-100 text-gray-800",
    success: "bg-green-100 text-green-800",
    warning: "bg-yellow-100 text-yellow-800",
    danger: "bg-red-100 text-red-800",
  };

  const sizeClasses = {
    sm: "px-2 py-0.5 text-xs",
    md: "px-2.5 py-0.5 text-sm",
    lg: "px-3 py-1 text-base",
  };

  const roundedClasses = props.rounded ? "rounded-full" : "rounded-md";

  return `${baseClasses} ${variantClasses[props.variant]} ${
    sizeClasses[props.size]
  } ${roundedClasses}`;
});
</script>
