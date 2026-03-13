<script setup>
definePage({
  meta: {
    navActiveLink: "front-pages-campaign",
  },
});
import { ref } from "vue";
import { useRouter } from "vue-router";

import StepCampaignSetup from "./components/standard/StepCampaignSetup.vue";
import StepTargetAudience from "./components/standard/StepTargetAudience.vue";
import StepWorkflowBuilder from "./components/standard/StepWorkflowBuilder.vue";

const router = useRouter();

const step = ref(1);

/* STORE STEP DATA */

const campaignData = ref({});
const audienceData = ref({});
const workflowData = ref({});

/* STEP 1 → NEXT */

const handleStep1 = (data) => {
  campaignData.value = data;
  step.value = 2;
};

/* STEP 2 → NEXT */

const handleStep2 = (data) => {
  audienceData.value = data;
  step.value = 3;
};

/* BACK */

const prevStep = () => {
  step.value--;
};

/* SAVE DRAFT */

const saveDraft = (data) => {
  workflowData.value = data;

  const draftCampaign = {
    id: Date.now(),
    campaign: campaignData.value,
    audience: audienceData.value,
    workflow: workflowData.value,
    status: "draft",
    createdAt: new Date(),
  };

  const campaigns = JSON.parse(localStorage.getItem("campaigns") || "[]");

  campaigns.push(draftCampaign);

  localStorage.setItem("campaigns", JSON.stringify(campaigns));

  alert("Draft Saved!");
};

/* LAUNCH CAMPAIGN */

const launchCampaign = (data) => {
  workflowData.value = data;

  const newCampaign = {
    id: Date.now(),
    campaign: campaignData.value,
    audience: audienceData.value,
    workflow: workflowData.value,
    status: "active",
    createdAt: new Date(),
  };

  const campaigns = JSON.parse(localStorage.getItem("campaigns") || "[]");

  campaigns.push(newCampaign);

  localStorage.setItem("campaigns", JSON.stringify(campaigns));

  /* redirect to campaign home */

  router.push("/front-pages/campaign");
};
</script>

<template>
  <!-- STEP 1 -->

  <StepCampaignSetup v-if="step === 1" @next="handleStep1" />

  <!-- STEP 2 -->

  <StepTargetAudience
    v-if="step === 2"
    @next="handleStep2"
    @back="prevStep"
    @saveDraft="saveDraft"
  />

  <!-- STEP 3 -->

  <StepWorkflowBuilder
    v-if="step === 3"
    @back="prevStep"
    @saveDraft="saveDraft"
    @launch="launchCampaign"
  />
</template>
