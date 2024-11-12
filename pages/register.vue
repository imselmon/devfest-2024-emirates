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
                    :disabled="!isFormValid"
                    @click="submitForm"
                >
                  Register
                </v-btn>
             </v-col>
            </v-row>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
  </NuxtLayout>
</template>

<script setup>
import { ref, computed } from 'vue';
import axios from 'axios';


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

const isFormValid = computed(() => {
  return name.value && email.value && phone.value && company.value && designation.value && location.value && expectations.value;
});

const submitForm = async () => {
  if (!isFormValid.value) {
    alert('Please fill all required fields.');
    return;
  }
  try {
    const response = await axios.post('https://script.google.com/macros/s/AKfycbyVkIOowVfJKsJtpkfVsYDGzP1Bdt3TpNxUtfRk8-F3uKy6VjArjoCrvf31RckDEFkw3g/exec', {
      name: name.value,
      email: email.value,
      phone: phone.value,
      company: company.value,
      designation: designation.value,
      location: location.value,
      linkedin: linkedin.value,
      github: github.value,
      expectations: expectations.value,
      newsletter: newsletter.value
    });
    console.log('Registration successful:', response.data);
  } catch (error) {
    console.error('Registration failed:', error);
  }
};
</script>

<style scoped>
/* Add any custom styles here */
</style>
