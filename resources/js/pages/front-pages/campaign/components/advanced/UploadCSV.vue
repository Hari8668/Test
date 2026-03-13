<script setup>
import { ref } from "vue";

const emit = defineEmits(["uploaded"]);

const file = ref(null);

const handleFile = (e) => {
  const selected = e.target.files[0];
  if (!selected) return;

  file.value = selected;

  emit("uploaded", selected);
};
</script>

<template>
  <VCard class="pa-6 mt-6">
    <h3 class="mb-4">
      Upload CSV File
      <span class="step">Step 1 of 2</span>
    </h3>

    <!-- Upload Area -->

    <div class="upload-box">
      <VIcon icon="tabler-upload" size="28" />

      <p class="mt-2">
        Drag a file or
        <span class="browse">click a browse</span>
      </p>

      <p class="text-caption">File with up to 100 rows works best</p>

      <input
        type="file"
        accept=".csv"
        @change="handleFile"
        class="file-input"
      />
    </div>

    <div class="mt-3 sample">
      <VIcon icon="tabler-download" size="16" />
      Download a sample file
    </div>
  </VCard>
</template>

<style scoped>
.step {
  font-size: 12px;
  margin-left: 10px;
  color: #9ca3af;
}

.upload-box {
  border: 2px dashed #6366f1;
  border-radius: 10px;
  padding: 50px;
  text-align: center;
  cursor: pointer;
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

.sample {
  font-size: 13px;
  color: #6366f1;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 6px;
}
</style>
