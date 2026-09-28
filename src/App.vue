<script setup>
import { ref, computed, watch } from "vue";
import UserCard from "./components/UserCard.vue";

const judul = "Dashboard Vue — Pertemuan 5";
const users = ref([]);
const keadaan = ref("idle"); // idle | loading | empty | error | success

async function muatPengguna() {
  keadaan.value = "loading";
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");
    if (!response.ok) {
      throw new Error("Status HTTP: " + response.status);
    }
    const data = await response.json();
    if (data.length === 0) {
      keadaan.value = "empty";
      return;
    }
    users.value = data;
    keadaan.value = "success";
  } catch (error) {
    console.error("Gagal memuat data:", error);
    keadaan.value = "error";
  }
}

const queryPencarian = ref("");
const riwayatPencarian = ref([]);
const penggunaTersaring = computed(() => {
  const q = queryPencarian.value.toLowerCase().trim();
  if (!q) return users.value;
  return users.value.filter((user) =>
    user.username.toLowerCase().includes(q)
  );
});

// Problem 2: sort berantai (hasil search diurutkan)
const arahUrutan = ref("asc"); // "asc" | "desc"
const penggunaTerurut = computed(() => {
  const salinan = [...penggunaTersaring.value];
  return salinan.sort((a, b) =>
    arahUrutan.value === "asc"
      ? a.name.localeCompare(b.name)
      : b.name.localeCompare(a.name)
  );
});

const jumlahHasil = computed(() => penggunaTersaring.value.length);

watch(queryPencarian, (nilaiBaru) => {
  if (nilaiBaru.trim() !== "") {
    riwayatPencarian.value.push(nilaiBaru);
  }
});
</script>

<template>
  <header class="app-header">
    <h1>{{ judul }}</h1>
  </header>
  <main class="app-main">
    <div class="toolbar">
      <button class="btn" @click="muatPengguna">Muat Pengguna</button>
      <input
        class="input-cari"
        v-model="queryPencarian"
        type="text"
        placeholder="Cari username..."
      />
      <button
        class="btn-urut"
        :class="{ 'btn-urut--aktif': arahUrutan === 'asc' }"
        @click="arahUrutan = 'asc'"
      >
        Urutkan A-Z
      </button>
      <button
        class="btn-urut"
        :class="{ 'btn-urut--aktif': arahUrutan === 'desc' }"
        @click="arahUrutan = 'desc'"
      >
        Urutkan Z-A
      </button>
    </div>
    <p>Menampilkan {{ jumlahHasil }} dari {{ users.length }} pengguna</p>
    <p v-if="keadaan === 'loading'">Memuat data...</p>
    <p v-else-if="keadaan === 'empty'">Tidak ada pengguna ditemukan.</p>
    <p v-else-if="keadaan === 'error'">Gagal memuat data. Coba lagi.</p>
    <ul v-else-if="keadaan === 'success'">
      <UserCard
        v-for="user in penggunaTerurut"
        :key="user.id"
        :user="user"
        :sorotan="user.username.toLowerCase() === queryPencarian.toLowerCase().trim()"
      />
    </ul>
  </main>
</template>

<style scoped>
.app-header {
  padding: 1.5rem;
  background: #0b4f6c;
  color: white;
}
.app-main {
  padding: 1.5rem;
}
.toolbar {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  align-items: center;
}
.btn {
  padding: 0.25rem 0.8rem;
  border: 1px solid #0b4f6c;
  border-radius: 6px;
  background: #0b4f6c;
  color: white;
  cursor: pointer;
}
.input-cari {
  padding: 0.3rem 0.6rem;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
}
.btn-urut {
  padding: 0.25rem 0.8rem;
  border: 1px solid #0b4f6c;
  border-radius: 6px;
  background: white;
  color: #0b4f6c;
  cursor: pointer;
}
.btn-urut--aktif {
  background: #0b4f6c;
  color: white;
}
</style>