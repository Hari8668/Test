<script setup>
import { ref, computed } from "vue";
import LookalikeDialog from "./LookalikeDialog.vue";

const emit = defineEmits(["next"]);

const selected = ref(null);
const linkedinUrl = ref("");
const csvFile = ref(null);
const webhookUrl = ref("");
const lookalikeKeyword = ref("");
const innerStep = ref(1);

const csvHeaders = ref([]);
const csvUploaded = ref(false);

const showLookalikeDialog = ref(false);
const selectedLookalike = ref(null);
const steps = ["linkedin", "csv", "lookalike", "webhook"];
/* SELECT METHOD */

const selectMethod = (method) => {
  selected.value = method;
  innerStep.value = steps.indexOf(method) + 1;
};

const handleCSVUpload = (event) => {
  const file = event.target.files[0];
  if (!file) return;

  csvFile.value = file;

  const reader = new FileReader();

  reader.onload = (e) => {
    const text = e.target.result;

    const rows = text.split("\n");
    const headers = rows[0].split(",");

    csvHeaders.value = headers.map((h) => h.trim());

    csvUploaded.value = true;
  };

  reader.readAsText(file);
};

/* VALIDATION */

const isValid = computed(() => {
  if (selected.value === "linkedin") return linkedinUrl.value.trim();
  if (selected.value === "csv") return csvUploaded.value;
  if (selected.value === "lookalike") return selectedLookalike.value;
  if (selected.value === "webhook") return webhookUrl.value;
  return false;
});

/* NEXT */

const next = () => {
  if (!isValid.value) return;

  if (innerStep.value < 4) {
    innerStep.value++;
    selected.value = ["linkedin", "csv", "lookalike", "webhook"][
      innerStep.value - 1
    ];
  } else {
    emit("next");
  }
};
</script>

<template>
  <VCard class="pa-6 import-wrapper">
    <!-- STEP HEADER -->

    <div class="stepper mb-6">
      <div class="step active">
        <VIcon icon="tabler-list-details" />
        <span>Define Target Audience</span>
      </div>

      <VIcon icon="tabler-chevron-right" />

      <div class="step">
        <VIcon icon="tabler-user" />
        <span>Sender Profiles</span>
      </div>
    </div>

    <!-- TIMELINE -->

    <div class="timeline"></div>
    <div class="timeline-dot"></div>

    <!-- IMPORT METHOD -->

    <h3 class="mb-4">Choose Import Method</h3>

    <VRow>
      <!-- LINKEDIN -->

      <VCol cols="12" md="3">
        <VCard
          class="import-card"
          :class="{ active: selected === 'linkedin' }"
          @click="selectMethod('linkedin')"
        >
          <VIcon icon="tabler-brand-linkedin" />

          <h4>LinkedIn Search</h4>

          <p>(Basic, Sales Nav, Post, Group or Event URL)</p>

          <VIcon
            v-if="selected === 'linkedin'"
            icon="tabler-check"
            class="check"
          />
        </VCard>
      </VCol>

      <!-- CSV -->

      <VCol cols="12" md="3">
        <VCard
          class="import-card"
          :class="{ active: selected === 'csv' }"
          @click="selectMethod('csv')"
        >
          <VIcon icon="tabler-upload" />

          <h4>Upload CSV File</h4>

          <p>
            Upload LinkedIn profiles via CSV.
            <span class="link">Download Sample</span>
          </p>

          <VIcon v-if="selected === 'csv'" icon="tabler-check" class="check" />
        </VCard>
      </VCol>

      <!-- LOOKALIKE -->

      <VCol cols="12" md="3">
        <VCard
          class="import-card"
          :class="{ active: selected === 'lookalike' }"
          @click="showLookalikeDialog = true"
        >
          <VIcon icon="tabler-users" />

          <h4>Lookalike Audience</h4>

          <p>Use Lead Finder to find audience.</p>

          <VIcon
            v-if="selected === 'lookalike'"
            icon="tabler-check"
            class="check"
          />
        </VCard>
      </VCol>

      <!-- WEBHOOK -->

      <VCol cols="12" md="3">
        <VCard
          class="import-card"
          :class="{ active: selected === 'webhook' }"
          @click="selectMethod('webhook')"
        >
          <VIcon icon="tabler-link" />

          <h4>Inbound Webhook</h4>

          <p>Sync leads from zapier, n8n make in real time</p>

          <VIcon
            v-if="selected === 'webhook'"
            icon="tabler-check"
            class="check"
          />
        </VCard>
      </VCol>
    </VRow>

    <!-- LINKEDIN SECTION -->

    <VCard v-if="selected === 'linkedin'" class="pa-4 mt-6">
      <div class="helper">
        <VIcon icon="tabler-brand-linkedin" size="18" />

        Find your target audience with
        <span class="link">LinkedIn Search</span>
        or
        <span class="link">Sales Navigator</span>
        or
        <span class="link">Post URL</span>
        or
        <span class="link">Group URL</span>

        <span class="guide">Search Guide</span>
      </div>

      <VRow class="mt-4" align="center">
        <VCol cols="12" md="9">
          <VTextField
            v-model="linkedinUrl"
            placeholder="https://www.linkedin.com/search/results/people/?keywords="
            variant="outlined"
            density="comfortable"
          />
        </VCol>

        <VCol cols="12" md="3">
          <VBtn color="primary" block> Validate </VBtn>
        </VCol>
      </VRow>
    </VCard>

    <!-- CSV STEP 1 -->

    <VCard v-if="selected === 'csv' && !csvUploaded" class="pa-6 mt-6">
      <h3 class="mb-4">
        Upload CSV File
        <span class="step-label">Step 1 of 2</span>
      </h3>

      <div class="upload-box">
        <VIcon icon="tabler-upload" size="30" />

        <p class="mt-2">
          Drag a file or <span class="browse">click a browse</span>
        </p>

        <p class="text-caption">File with up to 100 rows works best</p>

        <input
          type="file"
          accept=".csv"
          class="file-input"
          @change="handleCSVUpload"
        />
      </div>

      <p class="download mt-3">
        <VIcon icon="tabler-download" size="16" />
        Download a sample file
      </p>
    </VCard>

    <!-- CSV STEP 2 -->

    <VCard v-if="selected === 'csv' && csvUploaded" class="pa-6 mt-6">
      <h3 class="mb-4">
        Map Properties
        <span class="step-label">Step 2 of 2</span>
      </h3>

      <VRow>
        <VCol cols="12" md="8">
          <VRow
            v-for="item in [
              'Full name',
              'First name',
              'Last name',
              'Company Name',
              'Position',
              'Headline',
            ]"
            :key="item"
            class="mb-3"
          >
            <VCol cols="6">
              <VTextField :model-value="item" readonly variant="outlined" />
            </VCol>

            <VCol cols="6">
              <VSelect
                :items="[
                  'Full name',
                  'First name',
                  'Last name',
                  'Company Name',
                  'Position',
                  'Headline',
                ]"
                :model-value="item"
                variant="outlined"
              />
            </VCol>
          </VRow>
        </VCol>

        <VCol cols="12" md="4">
          <VCard class="pa-4">
            <h4 class="mb-3">Unmapped Works</h4>

            <VTextField
              placeholder="Search"
              prepend-inner-icon="tabler-search"
              variant="outlined"
              density="compact"
            />

            <div class="unmapped">Location</div>
            <div class="unmapped">Industry</div>
            <div class="unmapped">Notes</div>

            <p class="clear">Clear All Matched</p>
          </VCard>
        </VCol>
      </VRow>
    </VCard>

    <!-- LOOKALIKE -->

    <VCard v-if="selected === 'lookalike'" class="pa-4 mt-6">
      <VTextField label="Keyword / Company" v-model="lookalikeKeyword" />
    </VCard>

    <!-- WEBHOOK -->

    <VCard v-if="selected === 'webhook'" class="pa-4 mt-6">
      <VTextField label="Webhook URL" v-model="webhookUrl" />
    </VCard>

    <!-- FOOTER -->

    <div class="footer">
      <VBtn color="primary" :disabled="!isValid" @click="next"> Next </VBtn>
    </div>

    <!-- LOOKALIKE DIALOG -->

    <LookalikeDialog
      v-if="showLookalikeDialog"
      @close="showLookalikeDialog = false"
      @select="
        (list) => {
          selectedLookalike = list;
          selected = 'lookalike';
          showLookalikeDialog = false;
        }
      "
    />
  </VCard>
</template>

<style scoped>
.import-wrapper {
  position: relative;
  padding-left: 28px;
}

.timeline {
  position: absolute;
  left: 0;
  top: 100px;
  bottom: 0;
  width: 2px;
  background: #e5e7eb;
}

.timeline-dot {
  position: absolute;
  left: -5px;
  top: 95px;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 3px solid #6366f1;
  background: white;
}

.stepper {
  display: flex;
  align-items: center;
  gap: 10px;
}

.step {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #9ca3af;
}

.step.active {
  color: #6366f1;
  font-weight: 600;
}

.import-card {
  padding: 18px;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  cursor: pointer;
  position: relative;
}

.import-card.active {
  border: 2px solid #6366f1;
  background: #f5f7ff;
}

.check {
  position: absolute;
  top: 10px;
  right: 10px;
  background: #6366f1;
  color: white;
  border-radius: 6px;
  padding: 3px;
}

.link {
  color: #6366f1;
  cursor: pointer;
}

.helper {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 6px;
  font-size: 14px;
  line-height: 1.5;
}

.guide {
  margin-left: auto;
  font-size: 13px;
  color: #6366f1;
}

.footer {
  display: flex;
  justify-content: flex-end;
  margin-top: 30px;
}

.upload-box {
  border: 2px dashed #6366f1;
  border-radius: 10px;
  padding: 50px;
  text-align: center;
  position: relative;
}

.file-input {
  position: absolute;
  inset: 0;
  opacity: 0;
  cursor: pointer;
}

.browse {
  color: #6366f1;
  font-weight: 500;
}

.download {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #6366f1;
  cursor: pointer;
}

.unmapped {
  border: 1px solid #e5e7eb;
  padding: 8px;
  border-radius: 6px;
  margin-top: 8px;
}

.clear {
  margin-top: 10px;
  font-size: 13px;
  color: #6366f1;
  cursor: pointer;
}
</style>
