<script setup>
import { ref } from "vue";

const emit = defineEmits(["previous", "submit"]);

const tab = ref("linkedin");

const profiles = ref([
  {
    name: "Edgar Jones",
    connections: "1,250 connections",
    health: 72,
    daily: "Invites: 40 / day",
    type: "Premium",
    status: "Connected",
  },
]);
</script>

<template>
  <VCard class="pa-6">
    <!-- STEP HEADER -->

    <div class="stepper mb-6">
      <div class="step done">
        <VIcon icon="tabler-list-details" />
        Define Target Audience
      </div>

      <VIcon icon="tabler-chevron-right" />

      <div class="step active">
        <VIcon icon="tabler-user" />
        Sender Profiles
      </div>
    </div>

    <!-- TABS -->

    <VTabs v-model="tab" class="mb-4">
      <VTab value="linkedin">LinkedIn Profile</VTab>
      <VTab value="email">Email Accounts</VTab>
    </VTabs>

    <!-- LINKEDIN TABLE -->

    <VCard v-if="tab === 'linkedin'" class="pa-4">
      <!-- HEADER -->

      <div class="table-header">
        <div>
          <h3>LinkedIn Profile</h3>

          <p class="text-caption">
            Pick which Linkedin profiles you want to use for this campaign.
          </p>
        </div>

        <VBtn color="primary">+ Add Account</VBtn>
      </div>

      <!-- FILTER -->

      <div class="table-tools">
        <VSelect label="Show" :items="[10, 25, 50]" density="compact" />

        <VTextField
          placeholder="Search"
          prepend-inner-icon="tabler-search"
          density="compact"
        />
      </div>

      <!-- TABLE -->

      <div class="table-wrapper">
        <VTable>
          <thead>
            <tr>
              <th></th>
              <th>NAME</th>
              <th>HEALTH</th>
              <th>DAILY LIMITS</th>
              <th>ACCOUNT TYPE</th>
              <th>STATUS</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="profile in profiles" :key="profile.name">
              <td>
                <VCheckbox />
              </td>

              <td>
                <div class="profile">
                  <VAvatar size="32" image="https://i.pravatar.cc/40" />

                  <div class="ml-2">
                    <strong>{{ profile.name }}</strong>

                    <p class="text-caption">
                      {{ profile.connections }}
                    </p>
                  </div>
                </div>
              </td>

              <td>
                <VProgressCircular
                  :size="40"
                  :width="4"
                  :value="profile.health"
                  color="orange"
                >
                  {{ profile.health }}
                </VProgressCircular>
              </td>

              <td>
                <VChip>{{ profile.daily }}</VChip>
              </td>

              <td>
                <VIcon icon="tabler-brand-linkedin" color="orange" />
                {{ profile.type }}
              </td>

              <td>
                <VChip color="green">{{ profile.status }}</VChip>
              </td>
            </tr>
          </tbody>
        </VTable>
      </div>
    </VCard>

    <!-- EMAIL TAB -->

    <VCard v-if="tab === 'email'" class="pa-6 text-center">
      <p>No email accounts connected</p>

      <VBtn color="primary" class="mt-3"> Add Email Account </VBtn>
    </VCard>

    <!-- FOOTER -->

    <div class="footer">
      <VBtn variant="text" @click="$emit('previous')">
        <VIcon icon="tabler-arrow-left" />
        Previous
      </VBtn>

      <VBtn color="primary" @click="$emit('submit')"> Submit </VBtn>
    </div>
  </VCard>
</template>

<style scoped>
/* STEP HEADER */

.stepper {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.step {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #9ca3af;
}

.step.done {
  color: #6366f1;
}

.step.active {
  color: #6366f1;
  font-weight: 600;
}

/* HEADER */

.table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  gap: 10px;
  flex-wrap: wrap;
}

/* FILTER */

.table-tools {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  gap: 10px;
  flex-wrap: wrap;
}

/* TABLE */

.table-wrapper {
  overflow-x: auto;
}

/* PROFILE CELL */

.profile {
  display: flex;
  align-items: center;
  gap: 8px;
}

/* FOOTER */

.footer {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
  flex-wrap: wrap;
  gap: 10px;
}

/* MOBILE */

@media (max-width: 768px) {
  .table-header {
    flex-direction: column;
    align-items: flex-start;
  }

  .table-tools {
    flex-direction: column;
  }

  .footer {
    flex-direction: column;
  }

  .footer .v-btn {
    width: 100%;
  }
}
</style>
