<script setup>
import { ref } from "vue";

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

const detailTerbuka = ref(false);
</script>

<template>
  <li class="user-card" :class="{ 'user-card--sorotan': sorotan }">
    <div class="user-card__baris">
      <div class="user-card__info">
        <strong>{{ user.name }}</strong>
        <span>{{ user.email }}</span>
      </div>
      <button
        class="user-card__tombol"
        :class="{ 'user-card__tombol--aktif': detailTerbuka }"
        @click="detailTerbuka = !detailTerbuka"
      >
        {{ detailTerbuka ? "Sembunyikan" : "Lihat Detail" }}
      </button>
    </div>

    <div v-if="detailTerbuka" class="user-card__detail">
      <p>Telepon: {{ user.phone }}</p>
      <p>Perusahaan: {{ user.company.name }}</p>
      <p>Kota: {{ user.address.city }}</p>
    </div>
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
.user-card__baris {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.user-card__info {
  display: flex;
  flex-direction: column;
  gap: 0.15rem;
}
.user-card__tombol {
  padding: 0.3rem 0.8rem;
  border: 1px solid #0b4f6c;
  border-radius: 6px;
  background: white;
  color: #0b4f6c;
  font-size: 0.8rem;
  cursor: pointer;
}
.user-card__tombol--aktif {
  background: #0b4f6c;
  color: white;
}
.user-card__detail {
  padding: 0.5rem 0.75rem;
  background: #f1f5f9;
  border-radius: 4px;
  font-size: 0.85rem;
}
.user-card__detail p {
  margin: 0.15rem 0;
}
</style>