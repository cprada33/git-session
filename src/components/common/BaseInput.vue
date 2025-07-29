<template>
  <div class="w-full">
    <label
      v-if="label"
      :for="id"
      class="block text-sm font-medium text-gray-700 mb-1">
      {{ label }}
      <span v-if="required" class="text-red-500">*</span>
    </label>

    <input
      :id="id"
      :type="type"
      :value="modelValue"
      :placeholder="placeholder"
      :disabled="disabled"
      :required="required"
      :class="inputClasses"
      @input="handleInput"
      @blur="handleBlur"
      @focus="handleFocus" />

    <p v-if="error" class="mt-1 text-sm text-red-600">
      {{ error }}
    </p>

    <p v-else-if="hint" class="mt-1 text-sm text-gray-500">
      {{ hint }}
    </p>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";

interface Props {
  modelValue?: string;
  type?: "text" | "email" | "password" | "number" | "tel" | "url" | "search";
  label?: string;
  placeholder?: string;
  hint?: string;
  error?: string;
  disabled?: boolean;
  required?: boolean;
  size?: "sm" | "md" | "lg";
  id?: string;
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: "",
  type: "text",
  disabled: false,
  required: false,
  size: "md",
});

const emit = defineEmits<{
  "update:modelValue": [value: string];
  blur: [event: FocusEvent];
  focus: [event: FocusEvent];
}>();

const id = computed(
  () => props.id || `input-${Math.random().toString(36).substr(2, 9)}`
);

const inputClasses = computed(() => {
  const baseClasses =
    "w-full border rounded-md transition-colors focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500";

  const sizeClasses = {
    sm: "px-3 py-1.5 text-sm",
    md: "px-4 py-2",
    lg: "px-4 py-3 text-lg",
  };

  const stateClasses = props.error
    ? "border-red-300 focus:ring-red-500 focus:border-red-500"
    : "border-gray-300";

  const disabledClasses = props.disabled
    ? "bg-gray-50 cursor-not-allowed opacity-50"
    : "";

  return `${baseClasses} ${
    sizeClasses[props.size]
  } ${stateClasses} ${disabledClasses}`;
});

const handleInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  emit("update:modelValue", target.value);
};

const handleBlur = (event: FocusEvent) => {
  emit("blur", event);
};

const handleFocus = (event: FocusEvent) => {
  emit("focus", event);
};
</script>
