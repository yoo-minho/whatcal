<script setup lang="ts">
import KnowledgeUser from "./components/KnowledgeUser.vue";

useAppConfig().ui.primary = "green";

const route = useRoute();
const id = +route.params.id[0];

const clickUser = (id: number) => {
  window.open(`https://www.inflearn.com/users/${id}`, id + "");
};
</script>
<template>
  <div class="m-3">
    <div class="flex gap-3 pb-3">
      <UButton color="white" variant="solid" @click="$router.push('/inf')" class="flex-1">
        <UIcon name="i-ph-arrow-left-light" size="1rem" />
        <div class="text-xs">다른 지식공유자 보기</div>
      </UButton>
      <UButton @click="clickUser(id)" variant="outline" class="flex-1 floating">
        <div class="flex justify-end items-center w-full gap-1">
          <div class="text-xs">인프런 페이지 이동</div>
          <UIcon name="i-ph-leaf-duotone" size="1rem" />
        </div>
      </UButton>
    </div>
    <KnowledgeUser :id="id" />
  </div>
</template>

<style lang="scss" scoped></style>
