<script setup>
import { ref } from 'vue';
import axios from 'axios';
import FormInput from '@/components/FormInput.vue';
import CityDropdown from '@/components/CityDropdown.vue';

// API Base URL
const apiBaseUrl = 'https://localhost:7121';

// Form fields
const formData = ref({
  name: '',
  email: '',
  age: '',
  city: ''
});

const isLoading = ref(false);
const showErrors = ref(false); // Controls when to show validation errors

// **Validation Computed Properties**
const emailError = () => {
  if (!formData.value.email) return "Email is required";
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return !emailPattern.test(formData.value.email) ? "Invalid email address" : "";
};

const ageError = () => {
  if (!formData.value.age) return "Age is required";
  if (isNaN(formData.value.age) || formData.value.age < 1 || formData.value.age > 120)
    return "Age must be a number between 1 and 120";
  return "";
};

// **Form validation check**
const isFormValid = () => {
  return formData.value.name &&
      formData.value.email &&
      !emailError() &&
      formData.value.age &&
      !ageError() &&
      formData.value.city;
};

// **Send API request**
const generateDocument = async () => {
  showErrors.value = true; // Show validation messages when button is clicked

  if (!isFormValid()) {
    return;
  }

  isLoading.value = true;

  try {
    // **Use provided API request code**
    const response = await axios.post(`${apiBaseUrl}/GenerateDocument`, formData.value, {
      responseType: 'blob' // Handle binary file response
    });

    // Create a download link
    const url = window.URL.createObjectURL(new Blob([response.data]));
    const link = document.createElement('a');
    link.href = url;
    link.setAttribute('download', 'GeneratedDocument.docx');
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  } catch (error) {
    console.error("Error generating document:", error);
    alert("Failed to generate document.");
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <div class="max-w-4xl mx-auto p-10 bg-white shadow-lg rounded-lg">
    <h2 class="text-2xl font-bold text-gray-700 mb-6">Generate Word Document</h2>

    <div class="space-y-4">
      <!-- Name Field -->
      <FormInput v-model="formData.name" label="Name" />
      <div v-if="showErrors && !formData.name" class="text-red-500 text-sm">Name is required</div>

      <!-- Email Field with Validation -->
      <div>
        <FormInput v-model="formData.email" label="Email" type="email" />
        <p v-if="showErrors && emailError()" class="text-red-500 text-sm mt-1">{{ emailError() }}</p>
      </div>

      <!-- Age Field with Validation -->
      <div>
        <FormInput v-model="formData.age" label="Age" type="number" />
        <p v-if="showErrors && ageError()" class="text-red-500 text-sm mt-1">{{ ageError() }}</p>
      </div>

      <!-- City Dropdown Component -->
      <CityDropdown v-model="formData.city" />
      <div v-if="showErrors && !formData.city" class="text-red-500 text-sm">City is required</div>
    </div>

    <!-- Generate Button -->
    <div class="mt-6">
      <button
          @click="generateDocument"
          class="relative bg-green-500 text-white px-6 py-3 rounded-lg hover:bg-green-600 flex items-center gap-2 w-full justify-center transition-all"
      >
        <svg v-if="isLoading" class="w-5 h-5 animate-spin text-white" fill="none" viewBox="0 0 24 24">
          <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
          <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8H4z"></path>
        </svg>
        <span>{{ isLoading ? 'Generating...' : 'Generate & Download' }}</span>
      </button>
    </div>
  </div>
</template>
