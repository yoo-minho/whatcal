<script setup lang="ts">
useAppConfig().ui.primary = "cyan";

const 연간구독매출 = ref(10);
const 월구독료 = ref(1);

const 필요유료고객수 = computed(() => Math.round((연간구독매출.value * 10000) / (월구독료.value * 12)));
const 필요모델고객수 = computed(() =>
  Math.round(필요유료고객수.value * (100 / items[selected.value].percent)).toLocaleString()
);

const page = ref<"intro" | "result">("intro");

const calculate = () => {
  page.value = "result";
};

const items = [
  {
    label: "부분 유료화",
    aka: "Freemium",
    content: "기본 서비스 무료, 프리미엄 기능 유료",
    percent: 4,
    expression: "전체고객수",
  },
  {
    label: "무료 체험판",
    aka: "Free Trial",
    content: "2~4주 체험판 제공, 계속 사용하려면 유료",
    percent: 8,
    expression: "체험고객수",
  },
];

const selected = ref(0);

const refers = [
  {
    title: "2024년 Micro SaaS에 주목하는 이유?",
    author: "기획하는 창업가",
    url: "https://brunch.co.kr/@firstevan/14",
  },
  {
    title: "Freemium and Free Trial Conversion Benchmarks",
    author: "Ada Chen Rekhi",
    url: "https://www.adachen.com/freemium-and-free-trial-conversion-benchmarks/",
  },
];

const title = "Micro SaaS";
const title2 = "고객 계산기";
const desc = `월 구독료 만원으로 연 10억 매출을 내려면\n고객이 얼마나 필요한지 계산해드립니다`;

const seoData = {
  title: `${title} ${title2}`,
  ogTitle: `${title} ${title2}`,
  description: desc,
  ogDescription: desc,
  twitterCard: "summary_large_image",
} as any;
useSeoStore(seoData);
useSeoStore({ title, title2, icon: "i-fluent-emoji-flat-microbe" });

defineOgImageComponent("LandingHero", { title, title2, desc, colorCode: "rgb(6,182,212)", chip: "💵👨‍💻👍" });
</script>
<template>
  <div class="m-3">
    <div class="w-full text-center">
      <p class="mt-3 text-sm tracking-tight text-gray-600 break-keep whitespace-pre">{{ desc }}</p>
    </div>

    <UDivider class="my-3" />

    <div class="flex flex-col gap-3">
      <template v-if="page === 'intro'">
        <BasicInput v-model="연간구독매출" label="연간 구독 매출" trailing="억 원" type="number">
          <template #tooltip> a.k.a ARR : Annual Recurring Revenue </template>
        </BasicInput>

        <BasicInput v-model="월구독료" label="1인당 월 구독료" trailing=" 만 원" type="number" />
        <BasicTab v-model="selected" label="모델에 따른 유료전환율" :items="items">
          <template #tooltip>
            <div class="text-sm">
              <span> 아래 자료를 활용하여<br />계산식에 활용할 유료전환율을 각각 정함 </span>
              <UDivider class="py-3" />
              <div class="font-bold tracking-tighter">Freemium and Free Trial Conversion Benchmarks</div>
              <div>by Ada Chen Rekhi</div>
              <a
                class="text-primary-600"
                href="https://www.adachen.com/freemium-and-free-trial-conversion-benchmarks/"
                target="Ada Chen Rekhi"
              >
                링크
              </a>
            </div>
          </template>
          <template #tab-contents="{ item }">
            <div class="flex flex-col">
              <span class="font-bold"> {{ item.content }} </span>
              <UDivider class="my-2" />
              <span>
                보통 전체 경험 고객 중 <b class="text-primary">{{ item.percent }}%</b> 유료 전환됨
              </span>
              <span>
                a.k.a <b class="text-primary">{{ item.aka }}</b>
              </span>
            </div>
          </template>
        </BasicTab>

        <UButton block color="primary" label="계산하기" size="lg" icon="i-heroicons-calculator" @click="calculate()" />
      </template>

      <template v-else-if="page === 'result'">
        <BasicLabel title="연간 구독 매출" :contents="연간구독매출 + '억 원'" />
        <BasicLabel title="1인당 월 구독료" :contents="월구독료 + '만 원'" />
        <BasicLabel title="모델" :contents="`${items[selected].label} (${items[selected].aka})`" />
        <UDivider />
        <BasicResultBanner>
          <template #name>유료고객수</template>
          <template #resultValue>
            <span class="text-primary">
              <span class="font-bold tracking-tighter">{{ 필요유료고객수.toLocaleString() }}</span>
              <span class="text-base ml-1">명</span>
            </span>
          </template>
        </BasicResultBanner>
        <BasicResultBanner>
          <template #name>{{ items[selected].expression }}</template>
          <template #resultValue>
            <span class="font-bold tracking-tighter">{{ 필요모델고객수 }}</span>
            <span class="text-base ml-1">명</span>
          </template>
        </BasicResultBanner>
        <UDivider type="dashed" />
        <UButton
          block
          color="primary"
          label="뒤로가기"
          size="lg"
          icon="i-heroicons-arrow-left-16-solid"
          @click="page = 'intro'"
        />
        <UDivider type="dashed" />
        <BasicRefer :refers="refers" />
      </template>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
