<script setup>
import { ref, computed } from "vue";
import CampaignStepper from "../common/CampaignStepper.vue";

const emit = defineEmits(["next", "back", "saveDraft"]);

const linkedinUrl = ref("");
const csvFile = ref(null);
const useWebhook = ref(false);
const fileInput = ref(null);

/* VALIDATION */

const isValid = computed(() => {
  return linkedinUrl.value || csvFile.value;
});

/* OPEN FILE PICKER */

const openFilePicker = () => {
  fileInput.value.click();
};

/* HANDLE FILE UPLOAD */

const handleFileUpload = (e) => {
  csvFile.value = e.target.files[0];
};

/* CONTINUE */

const handleNext = () => {
  const audienceData = {
    linkedinUrl: linkedinUrl.value,
    csvFile: csvFile.value,
    useWebhook: useWebhook.value,
  };

  emit("next", audienceData);
};

/* SAVE DRAFT */

const handleSaveDraft = () => {
  emit("saveDraft");
};
</script>

<template>
  <VCard class="pa-4 pa-md-8 campaign-card">
    <CampaignStepper :step="2" class="mb-6" />

    <VRow class="mt-2">
      <!-- LINKEDIN SEARCH -->

      <VCol cols="12" md="6" class="d-flex">
        <VCard class="pa-5 input-card w-100">
          <div class="card-title">Basic LinkedIn Search</div>

          <VTextField
            v-model="linkedinUrl"
            placeholder="https://www.linkedin.com/search/results/people/..."
            variant="outlined"
            density="comfortable"
          />

          <p class="helper-text">
            Filter profiles in the
            <a href="https://linkedin.com" target="_blank">LinkedIn search</a>
            and paste the URL above
          </p>
        </VCard>
      </VCol>

      <!-- CSV UPLOAD -->

      <VCol cols="12" md="3" class="d-flex">
        <VCard class="pa-5 input-card w-100">
          <div class="card-title">Upload CSV File</div>

          <input
            ref="fileInput"
            type="file"
            accept=".csv"
            hidden
            @change="handleFileUpload"
          />

          <div class="upload-box" @click="openFilePicker">
            <div class="upload-content">
              <VIcon icon="tabler-upload" size="34" class="mb-2" />

              <VBtn variant="outlined" size="small" class="mb-2">
                Choose File
              </VBtn>

              <p class="upload-text">Drop file here</p>

              <p v-if="csvFile" class="file-name">
                {{ csvFile.name }}
              </p>
            </div>
          </div>
        </VCard>
      </VCol>

      <!-- ADVANCED OPTIONS -->

      <VCol cols="12" md="3" class="d-flex">
        <VCard class="pa-5 input-card w-100">
          <div class="card-title">Advanced options</div>

          <VCheckbox v-model="useWebhook" label="Use Webhook" />

          <p class="helper-text">Use a webhook to send leads automatically.</p>
        </VCard>
      </VCol>
    </VRow>

    <!-- FOOTER -->

    <div class="footer">
      <VBtn variant="text" color="primary" @click="emit('back')"> Back </VBtn>

      <div class="footer-actions">
        <VBtn
          variant="outlined"
          color="primary"
          class="mr-md-3"
          @click="handleSaveDraft"
        >
          Save As Draft
        </VBtn>

        <VBtn color="primary" :disabled="!isValid" @click="handleNext">
          Continue
        </VBtn>
      </div>
    </div>
  </VCard>
</template>

<style scoped>
.campaign-card {
  border-radius: 12px;
}

/* INPUT CARDS */

.input-card {
  border: 1px solid #e5e7eb;
  border-radius: 10px;
}

/* CARD TITLE */

.card-title {
  font-weight: 600;
  margin-bottom: 12px;
}

/* HELPER TEXT */

.helper-text {
  font-size: 13px;
  color: #6b7280;
  margin-top: 6px;
}

/* CSV UPLOAD BOX */

.upload-box {
  border: 2px dashed #d1d5db;
  border-radius: 12px;
  height: 160px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
}

.upload-box:hover {
  border-color: #6366f1;
  background: #fafaff;
}

.upload-content {
  text-align: center;
}

.upload-text {
  font-size: 13px;
  color: #6b7280;
}

.file-name {
  margin-top: 6px;
  font-size: 12px;
  color: #6366f1;
  font-weight: 500;
}

/* FOOTER */

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 30px;
  flex-wrap: wrap;
  gap: 12px;
}

.footer-actions {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

/* MOBILE */

@media (max-width: 600px) {
  .footer {
    flex-direction: column;
    align-items: stretch;
  }

  .footer-actions {
    width: 100%;
    flex-direction: column;
  }

  .footer-actions .v-btn {
    width: 100%;
  }
}
</style>
