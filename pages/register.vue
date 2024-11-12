<template>
  <NuxtLayout name="default">
    <v-container fluid class="mt-5">
      <v-row>
        <v-col md="8">
          <h1>Register Form</h1>
          <p>
            Register now to get access to all the sessions, workshops, and networking opportunities.
            <br>
            <br>
            <strong>Registration is free and open to everyone.</strong>
          </p>
          <v-form>
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="name"
                  label="Name *"
                  required
                  rounded
                  borderless
                  style="border: none; "
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="email"
                  label="Email *"
                  required
                  rounded
                  style="border: none; "
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="phone"
                  label="WhatsApp Number *"
                  required
                  rounded
                  placeholder="e.g. +1234567890"
                  style="border: none; "
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="company"
                  label="Company / Institution *"
                  placeholder="e.g. RIT, Google, Microsoft, etc."
                  required
                  rounded
                  style="border: none; "
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="designation"
                  label="Designation *"
                  placeholder="e.g. Student, Developer, Designer, etc."
                  required
                  rounded
                  style="border: none; "
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="location"
                  label="Location *"
                  required
                  rounded
                  style="border: none; "
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="linkedin"
                  label="LinkedIn"
                  rounded
                  style="border: none; "
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="github"
                  label="GitHub"
                  rounded
                  style="border: none; "
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12">
                <v-textarea
                  v-model="expectations"
                  label="What do you expect out of the event? *"
                  required
                  rounded
                  style="border: none; "
                ></v-textarea>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12">
                <v-checkbox
                  v-model="newsletter"
                  label="Subscribe to our newsletter"
                ></v-checkbox>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12">
                <v-btn
                    class="mt-4 mb-5"
                    size="large"
                    color="#FFD427"
                    rounded
                    variant="flat"
                    style="border: 1.5px solid #1e1e1e; color: black;text-transform: capitalize"
                    id="submitForm"
                    :disabled="!isFormValid || loading"
                    @click="submitForm"
                >
                  <v-progress-circular
                    v-if="loading"
                    indeterminate
                    color="white"
                    size="20"
                  ></v-progress-circular>
                  <span v-else>Register</span>
                </v-btn>
             </v-col>
            </v-row>
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

const isFormValid = computed(() => {
  return name.value && email.value && phone.value && company.value && designation.value && location.value && expectations.value;
});

const submitForm = async () => {
  if (!isFormValid.value) {
    alert('Please fill all required fields.');
    return;
  }
  loading.value = true;
  const xhttp = new XMLHttpRequest();
  xhttp.open("POST", "https://script.google.com/macros/s/AKfycbxz6wp7fyHxqpmWIHE_1ev6dyiUO8J0imLPaON7IC57XDgTb6EdIrioB5mbWJnXCoxU/exec", true);
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

  const params = `name=${encodeURIComponent(name.value)}&email=${encodeURIComponent(email.value)}&phone=${encodeURIComponent(phone.value)}&company=${encodeURIComponent(company.value)}&designation=${encodeURIComponent(designation.value)}&location=${encodeURIComponent(location.value)}&linkedin=${encodeURIComponent(linkedin.value)}&github=${encodeURIComponent(github.value)}&expectations=${encodeURIComponent(expectations.value)}&newsletter=${encodeURIComponent(newsletter.value)}`;
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

<style scoped>
/* Add any custom styles here */
</style>
