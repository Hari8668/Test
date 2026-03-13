<script setup>
import { ref, onMounted } from "vue"

const campaigns = ref([])

onMounted(() => {

  const storedCampaigns = localStorage.getItem("campaigns")

  if (storedCampaigns) {
    campaigns.value = JSON.parse(storedCampaigns)
  }

})

const deleteCampaign = (id) => {

  campaigns.value = campaigns.value.filter(c => c.id !== id)

  localStorage.setItem("campaigns", JSON.stringify(campaigns.value))

}
</script>

<template>

<VCard class="pa-6">

<h2 class="text-h5 mb-6">
Campaigns
</h2>

<VTable>

<thead>

<tr>
<th>Name</th>
<th>Channel</th>
<th>Status</th>
<th>Created</th>
<th>Action</th>
</tr>

</thead>

<tbody>

<tr
v-for="campaign in campaigns"
:key="campaign.id"
>

<td>
{{ campaign.campaign.campaignName }}
</td>

<td>
{{ campaign.campaign.outreachChannel }}
</td>

<td>

<VChip
:color="campaign.status === 'active' ? 'success' : 'warning'"
>
{{ campaign.status }}
</VChip>

</td>

<td>
{{ new Date(campaign.createdAt).toLocaleDateString() }}
</td>

<td>

<VBtn
icon="tabler-trash"
variant="text"
color="error"
@click="deleteCampaign(campaign.id)"
/>

</td>

</tr>

</tbody>

</VTable>

</VCard>

</template>