<script setup lang="ts">
const page = useCookie("calcreator-page");
const nonReactivePage = [...(page.value || "")].join(""); //useCookie 반응성 이슈

if (nonReactivePage === "" || nonReactivePage === "/") {
  await navigateTo("/main", { replace: true });
} else {
  await navigateTo(nonReactivePage, { replace: true });
}

const title = "칼크리에이터";
const title2 = "calcreator.cc";
const desc = "계산기로 널리 이롭게";

useSeoMeta({
  title: `${title} ${title2}`,
  ogTitle: `${title} ${title2}`,
  description: desc,
  ogDescription: desc,
  twitterCard: "summary_large_image",
});

// 여기 주석 풀면 npm run build 시 The route is missing the Nuxt OG Image payload or route rules. 이슈남
// defineOgImageComponent("LandingHero", { chip: "🔢🌍👍" });
</script>
<template>
  <div></div>
</template>
