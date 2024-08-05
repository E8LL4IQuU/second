<template>
  <form @submit.prevent="saveToCookie">
    <label for="city">Город:</label>
    <select v-model="selectedCity" @change="updateShops">
      <option v-for="city in Object.keys(data.cities)" :key="city" :value="city">{{ city }}</option>
    </select>
    <br />

    <label for="shop">Цех:</label>
    <select v-model="selectedShop" @change="updateEmployees">
      <option v-for="shop in filteredShops" :key="shop" :value="shop">{{ shop }}</option>
    </select>
    <br />

    <label for="employee">Сотрудник:</label>
    <select v-model="selectedEmployee">
      <option v-for="employee in filteredEmployees" :key="employee" :value="employee">{{ employee }}</option>
    </select>
    <br />

    <label for="brigade">Бригада:</label>
    <select v-model="selectedBrigade">
      <option v-for="brigade in data.brigades" :key="brigade" :value="brigade">{{ brigade }}</option>
    </select>
    <br />

    <label for="shift">Смена:</label>
    <select v-model="selectedShift">
      <option v-for="shift in data.shifts" :key="shift" :value="shift">{{ shift }}</option>
    </select>
    <br />

    <button type="submit">Save</button>
  </form>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

interface Data {
  cities: Record<string, string[]>;
  shops: Record<string, string[]>;
  brigades: string[];
  shifts: string[];
}

const data: Data = {
  cities: {
    "City1": ["Shop1", "Shop2"],
    "City2": ["Shop3", "Shop4"]
  },
  shops: {
    "Shop1": ["Employee1", "Employee2"],
    "Shop2": ["Employee3", "Employee4"],
    "Shop3": ["Employee5", "Employee6"],
    "Shop4": ["Employee7", "Employee8"]
  },
  brigades: ["Brigade1", "Brigade2"],
  shifts: ["Shift1", "Shift2"]
};

const selectedCity = ref("City1");
const selectedShop = ref<string | null>(null);
const selectedEmployee = ref<string | null>(null);
const selectedBrigade = ref<string | null>(null);
const selectedShift = ref<string | null>(null);

const filteredShops = computed(() => {
  return data.cities[selectedCity.value] || [];
});

const filteredEmployees = computed(() => {
  return data.shops[selectedShop.value || ''] || [];
});

const updateShops = () => {
  selectedShop.value = filteredShops.value[0] || null;
  updateEmployees();
};

const updateEmployees = () => {
  selectedEmployee.value = filteredEmployees.value[0] || null;
};

const saveToCookie = () => {
  const selectedValues = {
    city: selectedCity.value,
    shop: selectedShop.value,
    employee: selectedEmployee.value,
    brigade: selectedBrigade.value,
    shift: selectedShift.value
  };

  document.cookie = `formData=${JSON.stringify(selectedValues)};path=/;`;
  alert('Data saved to cookie');
};

// Initialize the form with default values
updateShops();
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  max-width: 300px;
  margin: auto;
}
label {
  margin-top: 10px;
}
button {
  margin-top: 20px;
}
</style>

