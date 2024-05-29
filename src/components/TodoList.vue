<template>
  <div class="flex justify-center">
    <div class="w-[500px] text-left">
      <p :class="{ 'line-through': props.todo.status === 2 }">
        {{ props.todo.title }}
      </p>
      <p>{{ props.todo.startDate }} ~ {{ props.todo.endDate }}</p>
    </div>
    <div class="button-area">
      <button
        @click="$router.push(`/detail/${props.todo.id}`)"
        class="py-2 px-3 rounded bg-green-700 text-white font-semibold"
      >
        詳細
      </button>
      <button
        v-if="props.todo.status === 0"
        class="py-2 px-3 border border-solid rounded"
        @click="updateStatus()"
      >
        未着手
      </button>
      <button
        v-else-if="props.todo.status === 1"
        class="py-2 px-3 bg-yellow-500 text-white font-semibold rounded"
        @click="updateStatus()"
      >
        作業中
      </button>
      <button
        v-else
        class="py-2 px-3 bg-red-700 text-white font-semibold rounded"
        @click="updateStatus()"
      >
        完了
      </button>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { Todo } from "@/interface/todo";

interface Props {
  todo: Todo;
}

const props = defineProps<Props>();

interface Emits {
  (e: "updateStatus", id: number, status: number): void;
}

const emit = defineEmits<Emits>();

/**
 * ステータス更新
 */
const updateStatus = (): void => {
  emit("updateStatus", props.todo.id, props.todo.status);
};
</script>
<style lang="postcss" scoped>
button {
  @apply w-[80px] mx-2;
}

p {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>
