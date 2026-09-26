<script setup>
defineProps({
  user: {
    type: Object,
    required: true,
  },
  sorotan: {
    type: Boolean,
    default: false,
  },
});
</script>

<template>
  <li class="user-card" :class="{ 'user-card--sorotan': sorotan }">
    <strong>{{ user.name }}</strong>
    <span>{{ user.email }}</span>
  </li>
</template>

<style scoped>
.user-card {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  padding: 0.75rem 1rem;
  border-bottom: 1px solid #e0e0e0;
}
.user-card--sorotan {
  background: #fff3cd;
}
</style>