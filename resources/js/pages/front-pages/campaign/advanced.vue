<script setup>
definePage({
  meta: {
    navActiveLink: "front-pages-campaign",
    layout: "default",
  },
});
import { useRoute } from "vue-router";
const route = useRoute();
console.log("route name:", route.name);
console.log("route meta:", route.meta);
import { ref } from "vue";

import ImportMethod from "./components/advanced/ImportMethod.vue";
import SenderProfiles from "./components/advanced/SenderProfiles.vue";

const step = ref(1);

/* NEXT */

const nextStep = () => {
  step.value = 2;
};

/* BACK */

const previousStep = () => {
  step.value = 1;
};

/* SUBMIT */

const submitCampaign = () => {
  console.log("Campaign submitted");
};
</script>

<template>
  <!-- STEP 1 -->

  <ImportMethod v-if="step === 1" @next="nextStep" />

  <!-- STEP 2 -->

  <SenderProfiles
    v-if="step === 2"
    @previous="previousStep"
    @submit="submitCampaign"
  />
</template>
