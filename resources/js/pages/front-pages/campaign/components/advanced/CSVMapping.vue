<script setup>
import { ref } from "vue";

const emit = defineEmits(["next"]);

const fields = ref([
  "Full name",
  "First name",
  "Last name",
  "Company Name",
  "Position",
  "Headline",
]);

const csvColumns = ref([
  "Full name",
  "First name",
  "Last name",
  "Company Name",
  "Position",
  "Headline",
]);

const unmapped = ref(["Location", "Industry", "Notes"]);

const goNext = () => {
  emit("next");
};
</script>

<template>
  <VCard class="pa-6 mt-6">
    <h3 class="mb-4">Map Properties</h3>

    <p class="text-caption mb-6">
      ✔ Make sure file includes contact name and phone number
    </p>

    <VRow>
      <!-- MAPPING -->

      <VCol cols="12" md="8">
        <VRow v-for="(field, index) in fields" :key="index" class="mb-3">
          <VCol cols="6">
            <VTextField :model-value="field" variant="outlined" readonly />
          </VCol>

          <VCol cols="6">
            <VSelect
              :items="csvColumns"
              :model-value="csvColumns[index]"
              variant="outlined"
            />
          </VCol>
        </VRow>
      </VCol>

      <!-- UNMAPPED -->

      <VCol cols="12" md="4">
        <VCard class="pa-4 unmapped-box">
          <h4 class="mb-3">Unmapped Works</h4>

          <VTextField
            placeholder="Search"
            prepend-inner-icon="tabler-search"
            variant="outlined"
            density="compact"
          />

          <div v-for="item in unmapped" :key="item" class="unmapped-item">
            {{ item }}
          </div>

          <div class="clear">Clear All Matched</div>
        </VCard>
      </VCol>
    </VRow>

    <div class="footer">
      <VBtn color="primary" @click="goNext"> Next </VBtn>
    </div>
  </VCard>
</template>

<style scoped>
.unmapped-box {
  border: 1px solid #e5e7eb;
}

.unmapped-item {
  border: 1px solid #e5e7eb;
  padding: 8px;
  border-radius: 6px;
  margin-top: 8px;
}

.clear {
  margin-top: 10px;
  color: #6366f1;
  font-size: 13px;
  cursor: pointer;
}

.footer {
  display: flex;
  justify-content: flex-end;
  margin-top: 20px;
}
</style>
