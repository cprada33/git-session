<template>
  <div :class="dividerClasses" :style="dividerStyles">
    <span v-if="text" :class="textClasses">
      {{ text }}
    </span>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";

interface Props {
  orientation?: "horizontal" | "vertical";
  text?: string;
  color?: "gray" | "blue" | "green" | "red";
  size?: "sm" | "md" | "lg";
}

const props = withDefaults(defineProps<Props>(), {
  orientation: "horizontal",
  variant: "solid",
  color: "gray",
  size: "md",
});

const dividerClasses = computed(() => {
  const baseClasses = "flex items-center";

  const orientationClasses = {
    horizontal: "w-full",
    vertical: "h-full flex-col",
  };

  const colorClasses = {
    gray: "border-gray-300",
    blue: "border-blue-300",
    green: "border-green-300",
    red: "border-red-300",
  };

  const variantClasses = {
    solid: "border-solid",
    dashed: "border-dashed",
    dotted: "border-dotted",
  };

  const sizeClasses = {
    sm: "border-t",
    md: "border-t-2",
    lg: "border-t-4",
  };

  const verticalSizeClasses = {
    sm: "border-l",
    md: "border-l-2",
    lg: "border-l-4",
  };

  const borderClass =
    props.orientation === "vertical"
      ? verticalSizeClasses[props.size]
      : sizeClasses[props.size];

  return `${baseClasses} ${orientationClasses[props.orientation]} ${
    colorClasses[props.color]
  } ${variantClasses[props.variant]} ${borderClass}`;
});

const dividerStyles = computed(() => {
  if (props.orientation === "vertical") {
    return {
      height: "100%",
      minHeight: "1rem",
    };
  }
  return {};
});

const textClasses = computed(() => {
  const baseClasses = "px-3 text-sm font-medium";

  const colorClasses = {
    gray: "text-gray-500 bg-white",
    blue: "text-blue-500 bg-white",
    green: "text-green-500 bg-white",
    red: "text-red-500 bg-white",
  };

  return `${baseClasses} ${colorClasses[props.color]}`;
});
</script>
