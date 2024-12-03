<script setup>
import { ref, computed } from 'vue';
const { mainData } = useJSONData();
definePageMeta({
  layout: false,
});

const name = ref('');
const email = ref('');
const phone = ref('');
const company = ref('');
const designation = ref('');
const location = ref('');
const linkedin = ref('');
const github = ref('');
const expectations = ref('');
const newsletter = ref(false);
const loading = ref(false);
const showModal = ref(false);
const foodPreference = ref(''); // Added foodPreference

const emailError = computed(() => {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return email.value && !emailPattern.test(email.value) ? 'Invalid email address' : '';
});

const phoneError = computed(() => {
  const phonePattern = /^\+?[1-9]\d{1,14}$/;
  return phone.value && !phonePattern.test(phone.value) ? 'Invalid phone number' : '';
});

const isFormValid = computed(() => {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  const phonePattern = /^\+?[1-9]\d{1,14}$/;
  return name.value && emailPattern.test(email.value) && phonePattern.test(phone.value) && company.value && designation.value && location.value && expectations.value && foodPreference.value;
});

const isRegistrationClosed = computed(() => {
  return new Date(mainData.eventInfo.registeration.end_date) <= new Date();
});

const submitForm = async () => {
  if (!isFormValid.value) {
    alert('Please fill all required fields.');
    return;
  }
  loading.value = true;
  const xhttp = new XMLHttpRequest();
  xhttp.open("POST", "https://script.google.com/macros/s/AKfycbz5wdvx-YrDk6vC3GEEYfZlcHcxXgK0qjq2IUcmM12lZh_n2RYvgVECJW1IFtT8IkNP/exec", true);
  xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
  xhttp.onreadystatechange = function() {
    if (xhttp.readyState === 4) {
      loading.value = false;
      if (xhttp.status === 200) {
        console.log('Registration successful:', xhttp.responseText);
        showModal.value = true;
      } else {
        console.error('Registration failed:', xhttp.responseText);
      }
    }
  };

  const params = `name=${encodeURIComponent(name.value)}&email=${encodeURIComponent(email.value)}&phone=${encodeURIComponent(phone.value)}&company=${encodeURIComponent(company.value)}&designation=${encodeURIComponent(designation.value)}&location=${encodeURIComponent(location.value)}&linkedin=${encodeURIComponent(linkedin.value)}&github=${encodeURIComponent(github.value)}&expectations=${encodeURIComponent(expectations.value)}&newsletter=${encodeURIComponent(newsletter.value)}&foodPreference=${encodeURIComponent(foodPreference.value)}`;
  xhttp.send(params);
};

useSeoMeta({
  contentType: "text/html; charset=utf-8",
  title:   `Registration  | ${mainData.eventInfo.name}` ,
  description: mainData.eventInfo.description.short,
  ogLocale:'en_US',
  keywords: mainData.seo.keywords,
  author: "OSS Labs",
  creator: "OSS Labs",
  viewport: "width=device-width, initial-scale=1.0",
  ogTitle: mainData.eventInfo.name + " | " + mainData.communityName,
  ogDescription: mainData.eventInfo.description.short,
  ogImage: `${mainData.seo.hostUrl}/thumbnail.png?auto=format&fit=crop&frame=1&h=512&w=1024`,
  ogUrl: mainData.seo.hostUrl,
  ogType: "website",
  twitterTitle: mainData.eventInfo.name + " | " + mainData.communityName,
  twitterDescription: mainData.eventInfo.description.short,
  twitterImage: `${mainData.seo.hostUrl}thumbnail.png?auto=format&fit=crop&frame=1&h=512&w=1024`,
  twitterCard: "summary_large_image",
});
</script>

<template>
  <NuxtLayout name="default">
    <v-container fluid class="mt-5">
      <v-row>
        <v-col md="8">
          <h1>Register Form</h1>
          <p v-if="!isRegistrationClosed">
            Register now to get access to all the sessions, workshops, and networking opportunities.
            <br>
            <br>
            <strong>Registration is free and open to everyone.</strong>
          </p>
          <p v-else>
            Registration is now closed. Thank you for your interest.
          </p>
          <v-form v-if="!isRegistrationClosed">
          </v-form>
        </v-col>
      </v-row>
    </v-container>
    <v-dialog v-model="showModal" max-width="500">
      <v-card>
        <v-card-title class="headline">Registration Successful</v-card-title>
        <v-card-text>
          Your registration has been successfully submitted.
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text
                 :href = "mainData.eventInfo.registeration.home"
                 @click="showModal = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </NuxtLayout>
</template>
