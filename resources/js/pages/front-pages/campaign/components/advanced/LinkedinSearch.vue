<script setup>
import { ref } from "vue";

const emit = defineEmits(["next", "back"]);

const selected = ref("linkedin");
const searchUrl = ref("");
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

    <div class="timeline">
      <div class="timeline-dot"></div>
    </div>

    <!-- IMPORT METHOD -->

    <h3 class="mb-4">Choose Import Method</h3>

    <VRow>
      <VCol cols="12" sm="6" md="3">
        <VCard class="import-card active">
          <VIcon icon="tabler-brand-linkedin" />

          <h4>LinkedIn Search</h4>

          <p>(Basic, Sales Nav, Post, Group or Event URL)</p>

          <VIcon class="check" icon="tabler-check" />
        </VCard>
      </VCol>

      <VCol cols="12" sm="6" md="3">
        <VCard class="import-card disabled">
          <VIcon icon="tabler-upload" />

          <h4>Upload CSV File</h4>

          <p>
            Upload LinkedIn profiles via CSV.
            <span class="link">Download Sample</span>
          </p>
        </VCard>
      </VCol>

      <VCol cols="12" sm="6" md="3">
        <VCard class="import-card disabled">
          <VIcon icon="tabler-users" />

          <h4>Lookalike Audience</h4>

          <p>Use Lead Finder to find audience.</p>
        </VCard>
      </VCol>

      <VCol cols="12" sm="6" md="3">
        <VCard class="import-card disabled">
          <VIcon icon="tabler-link" />

          <h4>Inbound Webhook</h4>

          <p>Sync leads from zapier, n8n make in real time</p>
        </VCard>
      </VCol>
    </VRow>

    <!-- LINKEDIN URL SECTION -->

    <div class="url-section mt-8">
      <div class="section-dot"></div>

      <h3 class="mb-4">Paste LinkedIn Search URL</h3>

      <VCard class="pa-4">
        <div class="helper-text mb-3">
          <VIcon icon="tabler-brand-linkedin" size="18" />

          Find your target audience with
          <span class="link">LinkedIn Search</span>
          or
          <span class="link">Sales </span>
          or
          <span class="link">Post URL</span>
          or
          <span class="link">Group URL</span>

          <span class="guide">Search Guide</span>
        </div>

        <VRow class="mt-4" align="center">
          <VCol cols="12" md="9">
            <VTextField
              v-model="searchUrl"
              placeholder="https://www.linkedin.com/search/results/people/?keywords="
              variant="outlined"
              density="comfortable"
            />
          </VCol>

          <VCol cols="12" md="3">
            <VBtn color="primary" block> Validate </VBtn>
          </VCol>
        </VRow>

        <p class="caption mt-2">Paste the search URL directly from LinkedIn</p>
      </VCard>
    </div>

    <!-- FOOTER -->

    <div class="footer">
      <VBtn color="primary" :disabled="!searchUrl" @click="$emit('next')">
        Next
      </VBtn>
    </div>
  </VCard>
</template>

<style scoped>
/* WRAPPER */

.import-wrapper {
  position: relative;
  padding-left: 28px;
}

/* STEP HEADER */

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

/* TIMELINE */

.timeline {
  position: absolute;
  left: 0;
  top: 80px;
  bottom: 0;
  width: 2px;
  background: #e5e7eb;
}

.timeline-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: white;
  border: 3px solid #22c55e;
  position: absolute;
  top: -30px;
  left: -6px;
}

/* IMPORT CARDS */

.import-card {
  padding: 16px;
  border: 1px solid #e5e7eb;
  border-radius: 10px;
  height: 150px;
  position: relative;
}

.import-card.active {
  border: 2px solid #6366f1;
  background: #f5f7ff;
}

.import-card.disabled {
  opacity: 0.5;
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

/* URL SECTION */

.url-section {
  position: relative;
}

.section-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 3px solid #6366f1;
  background: white;
  position: absolute;
  left: -22px;
  top: 10px;
}

/* TEXT */

.helper-text {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 14px;
}

.link {
  color: #6366f1;
  cursor: pointer;
}

.guide {
  margin-left: auto;
  color: #6366f1;
  font-size: 13px;
}

.caption {
  font-size: 12px;
  color: #6b7280;
}

/* FOOTER */

.footer {
  display: flex;
  justify-content: flex-end;
  margin-top: 30px;
}

/* RESPONSIVE */

@media (max-width: 768px) {
  .import-card {
    height: auto;
  }

  .helper-text {
    flex-wrap: wrap;
  }
}
</style>
