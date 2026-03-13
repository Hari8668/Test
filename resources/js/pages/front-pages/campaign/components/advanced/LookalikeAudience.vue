<script setup>
const emit = defineEmits(["next", "back"]);
</script>

<template>
  <VCard class="pa-6">
    <h3 class="mb-2">Lead Finder</h3>

    <p class="text-medium-emphasis">Search audience using filters</p>

    <VRow class="mt-6">
      <VCol cols="12" md="4">
        <VTextField label="Company" placeholder="Google" />
      </VCol>

      <VCol cols="12" md="4">
        <VTextField label="Job Title" placeholder="Founder" />
      </VCol>

      <VCol cols="12" md="4">
        <VTextField label="Location" placeholder="United States" />
      </VCol>
    </VRow>

    <VRow class="mt-2">
      <VCol cols="12" md="4">
        <VSelect
          label="Industry"
          :items="['Software', 'Finance', 'Marketing', 'Education']"
        />
      </VCol>

      <VCol cols="12" md="4">
        <VSelect
          label="Company Size"
          :items="['1-10', '11-50', '51-200', '200+']"
        />
      </VCol>

      <VCol cols="12" md="4">
        <VSelect
          label="Seniority"
          :items="['Founder', 'C-Level', 'Manager', 'Director']"
        />
      </VCol>
    </VRow>

    <div class="d-flex justify-space-between mt-8">
      <VBtn
        variant="text"
        prepend-icon="tabler-arrow-left"
        @click="$emit('back')"
      >
        Back
      </VBtn>

      <VBtn
        color="primary"
        append-icon="tabler-arrow-right"
        @click="$emit('next')"
      >
        Next
      </VBtn>
    </div>
  </VCard>
</template>
