<script setup>
const props = defineProps({
  step: Number
})
</script>

<template>
<div class="campaign-stepper">

  <!-- STEP 1 -->
  <div class="step" :class="{ active: step === 1, done: step > 1 }">
    <div class="icon">
      <VIcon icon="tabler-list-details" />
    </div>
    <span>Leads List</span>
  </div>

  <VIcon icon="tabler-chevron-right" class="arrow" />

  <!-- STEP 2 -->
  <div class="step" :class="{ active: step === 2, done: step > 2 }">
    <div class="icon">
      <VIcon icon="tabler-send" />
    </div>
    <span>Target Audience</span>
  </div>

  <VIcon icon="tabler-chevron-right" class="arrow" />

  <!-- STEP 3 -->
  <div class="step" :class="{ active: step === 3 }">
    <div class="icon">
      <VIcon icon="tabler-rocket" />
    </div>
    <span>Campaign</span>
  </div>

</div>
</template>

<style scoped>

.campaign-stepper{
display:flex;
align-items:center;
gap:16px;
padding:12px 16px;
border-radius:12px;
border:1px solid #e5e7eb;
background:#f9fafb;
font-weight:500;
flex-wrap:wrap;
}

/* STEP */

.step{
display:flex;
align-items:center;
gap:10px;
color:#9ca3af;
}

/* ICON */

.icon{
width:40px;
height:40px;
display:flex;
align-items:center;
justify-content:center;
border-radius:12px;
background:#eef2ff;
color:#9ca3af;
}

/* ACTIVE */

.step.active{
color:#6366f1;
font-weight:600;
}

.step.active .icon{
background:#6366f1;
color:white;
}

/* COMPLETED */

.step.done{
color:#6366f1;
}

.step.done .icon{
background:#6366f1;
color:white;
}

/* ARROW */

.arrow{
color:#9ca3af;
}

/* ---------- RESPONSIVE ---------- */

/* Tablet */

@media (max-width:900px){

.campaign-stepper{
gap:10px;
}

.step span{
font-size:13px;
}

.icon{
width:34px;
height:34px;
}

}

/* Mobile */

@media (max-width:600px){

.campaign-stepper{
flex-direction:column;
align-items:flex-start;
gap:12px;
}

.arrow{
display:none;
}

.step{
width:100%;
}

}

</style>