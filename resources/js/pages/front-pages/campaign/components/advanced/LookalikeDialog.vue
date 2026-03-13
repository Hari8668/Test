<script setup>
import { ref, computed } from "vue";

/*
Events:
close -> close dialog
select -> return selected list
*/

const emit = defineEmits(["close", "select"]);

/* Example lists (later replace with API data) */

const lists = ref([
  {
    name: "Founder",
    users: "1000+ Users in the List",
  },
  {
    name: "Tech Profiles",
    users: "1000+ Users in the List",
  },
]);

/* Selected index */

const selectedIndex = ref(null);

/* Check if lists exist */

const hasLists = computed(() => lists.value.length > 0);

/* Select list */

const selectList = (index) => {
  selectedIndex.value = index;
};

/* Confirm selection */

const confirmSelection = () => {
  if (selectedIndex.value !== null) {
    emit("select", lists.value[selectedIndex.value]);
  }
};
</script>

<template>
  <VDialog model-value="true" width="520" persistent>
    <VCard>
      <!-- HEADER -->

      <VCardTitle class="dialog-header">
        <div>
          <h3 class="title">Lookalikes</h3>

          <p class="subtitle">Select a lookalike list for this campaign</p>
        </div>

        <VBtn icon="tabler-x" variant="text" @click="$emit('close')" />
      </VCardTitle>

      <!-- BODY -->

      <VCardText>
        <!-- EMPTY STATE -->

        <div v-if="!hasLists" class="empty-state">
          <h2>You don’t have any leads</h2>

          <p class="empty-text">
            Create a lead list to start running campaigns
          </p>

          <VBtn color="primary"> Create a List </VBtn>
        </div>

        <!-- LISTS -->

        <div v-else>
          <VCard
            v-for="(list, index) in lists"
            :key="index"
            class="list-item mb-3"
            :class="{ active: selectedIndex === index }"
            @click="selectList(index)"
          >
            <VIcon icon="tabler-list" />

            <div class="ml-3">
              <strong>{{ list.name }}</strong>

              <p class="text-caption">
                {{ list.users }}
              </p>
            </div>

            <VSpacer />

            <VIcon
              v-if="selectedIndex === index"
              icon="tabler-check"
              color="primary"
            />
          </VCard>

          <p class="add-new">Add New</p>
        </div>
      </VCardText>

      <!-- FOOTER -->

      <VCardActions class="dialog-footer">
        <VBtn variant="text" @click="$emit('close')"> Cancel </VBtn>

        <VBtn
          color="primary"
          :disabled="selectedIndex === null"
          @click="confirmSelection"
        >
          Select List
        </VBtn>
      </VCardActions>
    </VCard>
  </VDialog>
</template>

<style scoped>
.dialog-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.title {
  font-size: 18px;
  font-weight: 600;
}

.subtitle {
  font-size: 13px;
  color: #9ca3af;
}

.empty-state {
  text-align: center;
  padding: 40px 10px;
}

.empty-text {
  margin: 10px 0 20px;
  color: #6b7280;
}

.list-item {
  display: flex;
  align-items: center;
  padding: 14px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.2s;
}

.list-item:hover {
  background: #f9fafb;
}

.list-item.active {
  border: 2px solid #6366f1;
  background: #f5f7ff;
}

.add-new {
  text-align: right;
  color: #6366f1;
  font-size: 13px;
  cursor: pointer;
  margin-top: 6px;
}

.dialog-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}
</style>
