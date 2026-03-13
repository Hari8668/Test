<script setup>
import { ref, computed } from "vue";
import CampaignStepper from "../common/CampaignStepper.vue";

const emit = defineEmits(["next"]);

const campaignName = ref("");
const campaignType = ref("LinkedIn Invitation");

const outreachChannel = ref("");

const startDay = ref("");
const endDay = ref("");
const startTime = ref("");
const endTime = ref("");

const days = [
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday",
  "Sunday",
];

const selectChannel = (channel) => {
  outreachChannel.value = channel;
};

/* FORM VALIDATION */

const isFormValid = computed(() => {
  return (
    campaignName.value &&
    campaignType.value &&
    outreachChannel.value &&
    startDay.value &&
    endDay.value &&
    startTime.value &&
    endTime.value
  );
});

/* SEND DATA */

const handleNext = () => {
  const campaignData = {
    campaignName: campaignName.value,
    campaignType: campaignType.value,
    outreachChannel: outreachChannel.value,
    schedule: {
      startDay: startDay.value,
      endDay: endDay.value,
      startTime: startTime.value,
      endTime: endTime.value,
    },
  };

  emit("next", campaignData);
};
</script>

<template>
  <VCard class="pa-4 pa-md-6">
    <CampaignStepper :step="1" class="mb-6" />

    <VRow>
      <!-- CAMPAIGN NAME -->

      <VCol cols="12" md="6">
        <VTextField
          v-model="campaignName"
          label="Campaign Name"
          placeholder="e.g. Founder Outreach – India"
        />

        <p class="text-caption text-medium-emphasis">
          Give your workflow a clear name so you can find it later.
        </p>
      </VCol>

      <!-- CAMPAIGN TYPE -->

      <VCol cols="12" md="6">
        <VSelect
          v-model="campaignType"
          :items="['LinkedIn Invitation', 'LinkedIn Message', 'Email']"
          label="Campaign Type"
        />

        <p class="text-caption text-medium-emphasis">
          Choose how this workflow will start.
        </p>
      </VCol>

      <!-- OUTREACH CHANNEL -->

      <VCol cols="12" md="6">
        <label class="text-body-2 font-weight-medium"> Outreach Channel </label>

        <div class="channel-buttons mt-2">
          <VBtn
            :color="outreachChannel === 'email' ? 'primary' : undefined"
            :variant="outreachChannel === 'email' ? 'flat' : 'outlined'"
            @click="selectChannel('email')"
          >
            Email Only
          </VBtn>

          <VBtn
            :color="outreachChannel === 'linkedin' ? 'primary' : undefined"
            :variant="outreachChannel === 'linkedin' ? 'flat' : 'outlined'"
            @click="selectChannel('linkedin')"
          >
            LinkedIn Only
          </VBtn>

          <VBtn
            :color="outreachChannel === 'both' ? 'primary' : undefined"
            :variant="outreachChannel === 'both' ? 'flat' : 'outlined'"
            @click="selectChannel('both')"
          >
            LinkedIn + Email
          </VBtn>
        </div>

        <p class="text-caption text-medium-emphasis mt-1">
          Select how you want to reach your leads in this workflow.
        </p>
      </VCol>

      <!-- SENDING SCHEDULE -->

      <VCol cols="12" md="6">
        <label class="text-body-2 font-weight-medium"> Sending Schedule </label>

        <VRow class="mt-2">
          <VCol cols="12" md="6">
            <VSelect v-model="startDay" :items="days" label="Start Day" />
          </VCol>

          <VCol cols="12" md="6">
            <VSelect v-model="endDay" :items="days" label="End Day" />
          </VCol>
        </VRow>

        <VRow class="mt-2">
          <VCol cols="12" md="6">
            <VTextField v-model="startTime" label="Start Time" type="time" />
          </VCol>

          <VCol cols="12" md="6">
            <VTextField v-model="endTime" label="End Time" type="time" />
          </VCol>
        </VRow>

        <p class="text-caption text-medium-emphasis mt-1">
          Messages are sent only within your selected schedule.
        </p>
      </VCol>
    </VRow>

    <!-- CONTINUE BUTTON -->

    <div class="footer mt-6">
      <VBtn color="primary" :disabled="!isFormValid" @click="handleNext">
        Continue
      </VBtn>
    </div>
  </VCard>
</template>

<style scoped>
/* CHANNEL BUTTONS */

.channel-buttons {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

/* SCHEDULE INPUT */

.schedule-input {
  flex: 1;
  min-width: 140px;
  margin-right: 8px;
}

/* FOOTER */

.footer {
  display: flex;
  justify-content: flex-end;
}

/* MOBILE FIX */

@media (max-width: 600px) {
  .footer {
    justify-content: stretch;
  }

  .footer .v-btn {
    width: 100%;
  }
}
</style>
