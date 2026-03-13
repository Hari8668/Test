<script setup>
import { ref, onMounted, computed } from "vue";

import WorkflowModeDialog from "./components/WorkflowModeDialog.vue";

const showDialog = ref(false);
const campaigns = ref([]);

/* OPEN NEW CAMPAIGN DIALOG */

const startCampaign = () => {
  showDialog.value = true;
};

/* SELECT MODE */

const selectMode = (selectedMode) => {
  showDialog.value = false;

  if (selectedMode === "standard") {
    window.open("/front-pages/campaign/standard", "_blank");
  } else {
    window.open("/front-pages/campaign/advanced", "_blank");
  }
};

/* LOAD CAMPAIGNS FROM LOCAL STORAGE */

onMounted(() => {
  const storedCampaigns = JSON.parse(localStorage.getItem("campaigns") || "[]");
  campaigns.value = storedCampaigns;
});

/* CHECK EMPTY STATE */

const hasCampaigns = computed(() => campaigns.value.length > 0);
</script>

<template>
  <!-- TOP FILTERS -->

  <VRow class="mb-6">
    <VCol cols="12" md="3">
      <VSelect
        label="Filter"
        :items="['All', 'Active', 'Draft']"
        variant="outlined"
      />
    </VCol>

    <VCol cols="12" md="4">
      <VTextField
        label="Search"
        prepend-inner-icon="tabler-search"
        variant="outlined"
      />
    </VCol>

    <!-- NEW CAMPAIGN BUTTON (VISIBLE ONLY IF CAMPAIGNS EXIST) -->

    <VCol
      v-if="hasCampaigns"
      cols="12"
      md="5"
      class="d-flex justify-end"
    >
      <VBtn color="primary" @click="startCampaign">
        New Campaign
      </VBtn>
    </VCol>
  </VRow>

  <!-- EMPTY STATE -->

  <div
    v-if="!hasCampaigns"
    class="d-flex flex-column align-center justify-center"
    style="height: 60vh"
  >
    <img src="/images/pages/empty-state.svg" style="width: 220px" />

    <VBtn class="mt-6" color="primary" @click="startCampaign">
      New Campaign
    </VBtn>
  </div>

  <!-- CAMPAIGN LIST -->

  <VRow v-else>
    <VCol
      v-for="campaign in campaigns"
      :key="campaign.id"
      cols="12"
      md="6"
      lg="4"
    >
      <VCard class="pa-4">

        <div class="d-flex justify-space-between align-center mb-2">
          <h3 class="text-subtitle-1 font-weight-bold">
            {{ campaign.campaign.campaignName }}
          </h3>

          <VChip
            :color="campaign.status === 'active' ? 'success' : 'warning'"
            size="small"
          >
            {{ campaign.status }}
          </VChip>
        </div>

        <p class="text-caption text-medium-emphasis">
          {{ campaign.campaign.campaignType }}
        </p>

        <p class="text-caption mt-2">
          {{ campaign.campaign.schedule.startDay }} →
          {{ campaign.campaign.schedule.endDay }}
        </p>

        <p class="text-caption">
          {{ campaign.campaign.schedule.startTime }} -
          {{ campaign.campaign.schedule.endTime }}
        </p>

      </VCard>
    </VCol>
  </VRow>

  <!-- WORKFLOW MODE DIALOG -->

  <WorkflowModeDialog
    v-if="showDialog"
    @select-mode="selectMode"
  />
</template>