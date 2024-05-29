<template>
  <div>
    <div class="top-area">
      <p class="top-title">予定詳細</p>
    </div>
    <div class="main-area">
      <div class="my-5">
        <input
          class="w-[500px]"
          placeholder="タイトル"
          type="text"
          v-model="title"
        />
      </div>
      <div class="my-5">
        <textarea
          class="w-[500px]"
          placeholder="内容"
          cols="30"
          rows="10"
          v-model="description"
        ></textarea>
      </div>
      <div class="flex justify-between my-5">
        <input class="w-[230px]" type="date" v-model="startDate" />
        <p>~</p>
        <input class="w-[230px]" type="date" v-model="endDate" />
      </div>
      <div class="my-5">
        <button class="update-button" @click="updateTodo">更新</button>
        <button class="delete-button" v-on:click="deleteTodo">削除</button>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { Todo } from "@/interface/todo";
import { ref } from "vue";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();

/**
 * ローカルストレージのデータを取得・保持
 */
const dataList = ref<Todo[]>(
  JSON.parse(window.localStorage.getItem("todoList") ?? "")
);
const data: Todo | undefined = dataList.value.find(
  (data: Todo) => data.id === Number(route.params.id)
);

const index: number = dataList.value.findIndex(
  (data: Todo) => data.id === Number(route.params.id)
);

const title = ref(data?.title);
const description = ref(data?.description);
const startDate = ref(data?.startDate);
const endDate = ref(data?.endDate);

/**
 * 予定の更新処理
 */
const updateTodo = () => {
  const dataset: Todo = {
    id: data?.id ?? 0,
    status: data?.status ?? 0,
    title: title?.value ?? "",
    description: description?.value ?? "",
    startDate: startDate?.value ?? "",
    endDate: endDate?.value ?? "",
  };
  dataList.value[index] = dataset;
  window.localStorage.setItem("todoList", JSON.stringify(dataList.value));
  router.push("/");
};
/**
 * 予定の削除処理
 */
const deleteTodo = () => {
  dataList.value.splice(index, 1);
  window.localStorage.setItem("todoList", JSON.stringify(dataList.value));
  router.push("/");
};
</script>
<style lang="postcss" scoped>
.top-area {
  @apply flex justify-center items-center my-7;
}
.main-area {
  @apply w-[500px] mx-auto;
}
.top-title {
  @apply my-5 text-2xl font-semibold;
}
input {
  @apply px-3 py-2 border border-solid border-gray-300 rounded outline-none;
}
textarea {
  @apply px-3 py-2 border border-solid border-gray-300 rounded outline-none resize-none;
}
.update-button {
  @apply bg-green-700 text-white;
}
.delete-button {
  @apply bg-red-700 text-white;
}

.update-button,
.delete-button {
  transition: all ease 0.2s;
  @apply font-semibold py-2 px-3 rounded mx-5 w-[100px];
}
</style>
