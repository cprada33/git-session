<template>
  <component :is="listType" :class="listClasses">
    <li v-for="(item, index) in items" :key="index" :class="itemClasses">
      <slot name="item" :item="item" :index="index">
        {{ item }}
      </slot>
    </li>
  </component>
</template>

<script setup lang="ts">
import { computed } from "vue";

interface Props {
  items?: any[];
  type?: "ul" | "ol";
  variant?: "default" | "bordered" | "striped" | "minimal";
  size?: "sm" | "md" | "lg";
  spacing?: "none" | "sm" | "md" | "lg";
  marker?:
    | "disc"
    | "circle"
    | "square"
    | "decimal"
    | "lower-alpha"
    | "upper-alpha"
    | "lower-roman"
    | "upper-roman";
}

const props = withDefaults(defineProps<Props>(), {
  items: () => [],
  type: "ul",
  variant: "default",
  size: "md",
  spacing: "md",
  marker: "disc",
});

const listType = computed(() => props.type);

const listClasses = computed(() => {
  const baseClasses = "list-none";

  const variantClasses = {
    default: "",
    bordered: "divide-y divide-gray-200",
    striped: "",
    minimal: "space-y-1",
  };

  const spacingClasses = {
    none: "",
    sm: "space-y-1",
    md: "space-y-2",
    lg: "space-y-3",
  };

  const markerClasses = {
    disc: "list-disc",
    circle: "list-circle",
    square: "list-square",
    decimal: "list-decimal",
    "lower-alpha": "list-[lower-alpha]",
    "upper-alpha": "list-[upper-alpha]",
    "lower-roman": "list-[lower-roman]",
    "upper-roman": "list-[upper-roman]",
  };

  // Only apply marker classes for ul/ol elements, not for custom styling
  const markerClass =
    props.variant === "minimal" ? "" : markerClasses[props.marker];

  return `${baseClasses} ${variantClasses[props.variant]} ${
    spacingClasses[props.spacing]
  } ${markerClass}`;
});

const itemClasses = computed(() => {
  const baseClasses = "";

  const sizeClasses = {
    sm: "text-sm",
    md: "text-base",
    lg: "text-lg",
  };

  const variantClasses = {
    default: "py-1",
    bordered: "py-3",
    striped: "py-2",
    minimal: "py-0.5",
  };

  const stripedClasses = props.variant === "striped" ? "even:bg-gray-50" : "";

  return `${baseClasses} ${sizeClasses[props.size]} ${
    variantClasses[props.variant]
  } ${stripedClasses}`;
});
</script>

<style scoped>
.list-disc {
  list-style-type: disc;
}

.list-circle {
  list-style-type: circle;
}

.list-square {
  list-style-type: square;
}

.list-decimal {
  list-style-type: decimal;
}

.list-\[lower-alpha\] {
  list-style-type: lower-alpha;
}

.list-\[upper-alpha\] {
  list-style-type: upper-alpha;
}

.list-\[lower-roman\] {
  list-style-type: lower-roman;
}

.list-\[upper-roman\] {
  list-style-type: upper-roman;
}
</style>
