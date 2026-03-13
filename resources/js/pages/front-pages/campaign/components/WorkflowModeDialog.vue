<script setup>
import { ref } from "vue"

const emit = defineEmits(["select-mode"])

const dialog = ref(true)
const selectedMode = ref("standard")

const nextStep = () => {
  emit("select-mode", selectedMode.value)
}
</script>

<template>
  <VDialog v-model="dialog" max-width="650">
    <VCard class="pa-6">

      <!-- HEADER -->

      <div class="d-flex justify-space-between align-center mb-4">
        <div>
          <h3 class="text-h6 font-weight-bold">Select Workflow Mode</h3>
          <p class="text-body-2 text-medium-emphasis">
            Choose how you want your campaign to behave
          </p>
        </div>

        <VBtn icon variant="text" @click="dialog = false">
          <VIcon icon="tabler-x" />
        </VBtn>
      </div>

      <!-- OPTIONS -->

      <VRadioGroup v-model="selectedMode">

        <!-- ADVANCED -->

        <VCard
          class="pa-4 mb-4 cursor-pointer"
          :class="selectedMode === 'advanced' ? 'border-primary' : 'border'"
          @click="selectedMode = 'advanced'"
        >
          <div class="d-flex justify-space-between align-center">

            <div class="d-flex align-center">
              <VRadio value="advanced" class="mr-3" />

              <div>
                <div class="d-flex align-center">
                  <strong>Advance Workflow</strong>

                  <VChip
                    color="success"
                    size="small"
                    class="ml-2"
                    label
                  >
                    Recommended
                  </VChip>
                </div>

                <p class="text-body-2 text-medium-emphasis">
                  Best for high-volume outreach
                </p>

                <div class="text-caption text-medium-emphasis">
                  • Conditional logic &nbsp;
                  • Multiple paths &nbsp;
                  • More control
                </div>
              </div>
            </div>

            <VIcon icon="tabler-layout-dashboard" size="36" />
          </div>
        </VCard>

        <!-- STANDARD -->

        <VCard
          class="pa-4 cursor-pointer"
          :class="selectedMode === 'standard' ? 'border-primary' : 'border'"
          @click="selectedMode = 'standard'"
        >
          <div class="d-flex justify-space-between align-center">

            <div class="d-flex align-center">
              <VRadio value="standard" class="mr-3" />

              <div>
                <strong>Standard Workflow</strong>

                <p class="text-body-2 text-medium-emphasis">
                  Best for beginners
                </p>

                <div class="text-caption text-medium-emphasis">
                  • Linear steps &nbsp;
                  • No conditions &nbsp;
                  • Easy setup
                </div>
              </div>
            </div>

            <VIcon icon="tabler-list-details" size="36" />
          </div>
        </VCard>

      </VRadioGroup>

      <!-- FOOTER -->

      <div class="d-flex justify-end mt-6">
        <VBtn variant="tonal" class="mr-2" @click="dialog = false">
          Close
        </VBtn>

        <VBtn color="primary" @click="nextStep">
          Next
        </VBtn>
      </div>

    </VCard>
  </VDialog>
</template>

<style scoped>
.border {
  border: 1px solid rgba(0,0,0,0.1);
}

.border-primary {
  border: 2px solid #696cff;
}
</style>