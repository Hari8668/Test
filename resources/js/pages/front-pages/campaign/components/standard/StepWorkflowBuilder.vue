<script setup>
import { ref } from "vue";
import CampaignStepper from "../common/CampaignStepper.vue";

const emit = defineEmits(["back", "saveDraft", "launch"]);

const connectionMessage = ref("");
const followupMessage = ref("");

const delayMinutes = ref(3);
const delayHours = ref(3);
const delayDays = ref(3);

/* BACK BUTTON */

const goBack = () => {
  emit("back");
};

/* SAVE DRAFT */

const saveDraft = () => {
  const workflowData = {
    connectionMessage: connectionMessage.value,
    followupMessage: followupMessage.value,
    delay: {
      minutes: delayMinutes.value,
      hours: delayHours.value,
      days: delayDays.value,
    },
  };

  emit("saveDraft", workflowData);
};

/* LAUNCH CAMPAIGN */

const launchCampaign = () => {
  const workflowData = {
    connectionMessage: connectionMessage.value,
    followupMessage: followupMessage.value,
    delay: {
      minutes: delayMinutes.value,
      hours: delayHours.value,
      days: delayDays.value,
    },
  };

  emit("launch", workflowData);
};
</script>

<template>
  <VCard class="pa-4 pa-md-6">
    <!-- Stepper -->

    <CampaignStepper :step="3" class="mb-6" />

    <div class="workflow">
      <!-- Campaign Start -->

      <div class="workflow-item">
        <div class="dot"></div>

        <VCard class="pa-4 workflow-card">
          <div class="workflow-title">
            <VIcon icon="tabler-rocket" class="mr-2" />

            Campaign Start
          </div>

          <p class="text-caption text-medium-emphasis">
            When a lead enters your target audience
          </p>
        </VCard>
      </div>

      <!-- LinkedIn Request -->

      <div class="workflow-item">
        <div class="dot"></div>

        <VCard class="pa-4 workflow-card">
          <div class="d-flex justify-space-between align-center mb-2">
            <div class="workflow-title">
              <VIcon icon="tabler-brand-linkedin" class="mr-2" />

              Send LinkedIn Connection Request
            </div>

            <div>
              <VIcon icon="tabler-edit" class="mr-2" />

              <VIcon icon="tabler-trash" color="error" />
            </div>
          </div>

          <VTextarea
            v-model="connectionMessage"
            placeholder="Hi {{first_name}}..."
            variant="outlined"
          />

          <div class="message-actions mt-3">
            <VBtn color="primary"> Edit Message </VBtn>

            <VBtn variant="outlined">
              <VIcon icon="tabler-sparkles" class="mr-1" />
              Make With AI
            </VBtn>
          </div>
        </VCard>
      </div>

      <!-- Follow Up -->

      <div class="workflow-item">
        <div class="dot"></div>

        <VCard class="pa-4 workflow-card">
          <div class="d-flex justify-space-between align-center mb-2">
            <div class="workflow-title">
              <VIcon icon="tabler-message" class="mr-2" />

              Set Follow-up message
            </div>

            <div>
              <VIcon icon="tabler-edit" class="mr-2" />

              <VIcon icon="tabler-trash" color="error" />
            </div>
          </div>

          <VTextarea
            v-model="followupMessage"
            placeholder="Hi {{first_name}}..."
            variant="outlined"
          />

          <div class="message-actions mt-3">
            <VBtn color="primary"> Edit Message </VBtn>

            <VBtn variant="outlined">
              <VIcon icon="tabler-sparkles" class="mr-1" />
              Make With AI
            </VBtn>
          </div>

          <!-- DELAY SETTINGS -->

          <div class="delay-box mt-4">
            <span class="delay-label"> Once accepted wait </span>

            <div class="delay-group">
              <VTextField
                v-model="delayMinutes"
                type="number"
                density="compact"
                label="Minutes"
                class="delay-input"
              />

              <VTextField
                v-model="delayHours"
                type="number"
                density="compact"
                label="Hours"
                class="delay-input"
              />

              <VTextField
                v-model="delayDays"
                type="number"
                density="compact"
                label="Days"
                class="delay-input"
              />
            </div>
          </div>
        </VCard>
      </div>

      <!-- Add Follow Up -->

      <div class="workflow-item">
        <div class="dot"></div>

        <div class="add-followup">
          <VIcon icon="tabler-plus" />

          Add new follow-up
        </div>
      </div>

      <!-- End Campaign -->

      <div class="workflow-item">
        <div class="dot"></div>

        <VCard class="pa-4 workflow-card">
          <VIcon icon="tabler-stop" class="mr-2" />

          End of Campaign
        </VCard>
      </div>
    </div>

    <!-- FOOTER BUTTONS -->

    <div class="footer mt-8">
      <VBtn variant="text" @click="goBack"> Back </VBtn>

      <div class="footer-actions">
        <VBtn variant="outlined" @click="saveDraft"> Save as Draft </VBtn>

        <VBtn color="primary" @click="launchCampaign"> Launch Campaign </VBtn>
      </div>
    </div>
  </VCard>
</template>

<style scoped>
/* WORKFLOW TIMELINE */

.workflow {
  position: relative;
  margin-left: 20px;
}

.workflow::before {
  content: "";
  position: absolute;
  left: -10px;
  top: 0;
  bottom: 0;
  width: 2px;
  background: #6366f1;
}

.workflow-item {
  position: relative;
  margin-bottom: 20px;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: white;
  border: 3px solid #6366f1;
  position: absolute;
  left: -16px;
  top: 18px;
}

/* CARD */

.workflow-card {
  border: 1px solid #e5e7eb;
  border-radius: 10px;
}

.workflow-title {
  font-weight: 600;
  display: flex;
  align-items: center;
}

/* DELAY INPUTS */

.delay-box {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.delay-label {
  font-size: 14px;
}

.delay-group {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.delay-input {
  width: 100px;
}

/* ADD FOLLOWUP */

.add-followup {
  border: 2px dashed #d1d5db;
  padding: 12px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  color: #6366f1;
  font-weight: 500;
}

.add-followup:hover {
  background: #f5f7ff;
}

/* FOOTER */

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
}

.footer-actions {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.message-actions {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

/* Mobile */

@media (max-width: 600px) {
  .message-actions {
    flex-direction: column;
  }

  .message-actions .v-btn {
    width: 100%;
  }
}
/* MOBILE */

@media (max-width: 768px) {
  .workflow {
    margin-left: 10px;
  }

  .workflow::before {
    left: -6px;
  }

  .dot {
    left: -12px;
  }

  .workflow-card {
    padding: 16px;
  }
}

@media (max-width: 600px) {
  .footer {
    flex-direction: column;
    align-items: stretch;
  }

  .footer-actions {
    flex-direction: column;
    width: 100%;
  }

  .footer-actions .v-btn {
    width: 100%;
  }
}
</style>
