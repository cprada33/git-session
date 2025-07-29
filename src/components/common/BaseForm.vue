<template>
  <form :class="formClasses" @submit.prevent="handleSubmit" novalidate>
    <slot />
  </form>
</template>

<script setup lang="ts">
import { computed } from "vue";

interface Props {
  layout?: "vertical" | "horizontal";
  spacing?: "sm" | "md" | "lg";
  disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  layout: "vertical",
  spacing: "md",
  disabled: false,
});

const emit = defineEmits<{
  submit: [formData: FormData];
  "submit-raw": [event: SubmitEvent];
}>();

const formClasses = computed(() => {
  const baseClasses = "w-full";

  const layoutClasses = {
    vertical: "space-y-4",
    horizontal: "space-y-6",
  };

  const spacingClasses = {
    sm: "space-y-2",
    md: "space-y-4",
    lg: "space-y-6",
  };

  return `${baseClasses} ${layoutClasses[props.layout]} ${
    spacingClasses[props.spacing]
  }`;
});

const handleSubmit = (event: Event) => {
  const submitEvent = event as SubmitEvent;

  if (props.disabled) {
    event.preventDefault();
    return;
  }

  emit("submit-raw", submitEvent);

  const formData = new FormData(event.target as HTMLFormElement);
  emit("submit", formData);
};
</script>
