<script lang="ts" setup>
import {
  confirmedValidator,
  emailValidator,
  passwordValidator,
  requiredValidator,
} from "@validators";
// import { ErrorMessage, Field, Form } from 'vee-validate';
import { computed, ref } from "vue";

// import type { VForm } from "vuetify/components";
// import * as yup from 'yup';
const name = ref("");
const email = ref("");
const password = ref("");
const confirmPassword = ref("");
const city = ref("");
const country = ref("");
const company = ref("");
const textareaValue = ref("");
const company1 = ref("");
const form = ref();
const form1 = ref();
const form2 = ref();
const currentStep = ref(0);
const isActive = ref(false);
const isActive3 = ref(false);

const currentSchema = computed(() => {
  // return schemas[currentStep.value];
});

async function nextStep() {
  if (currentStep.value == 0) {
    const res = await form.value.validate();
    if (res.valid) {
      currentStep.value = 1;
    } else {
      alert("error");
    }
  }
  if (currentStep.value == 1) {
    isActive.value = true;
    const res = await form1.value.validate();
    if (res.valid) {
      currentStep.value = 2;
    } else {
      alert("error");
    }
  }
  if (currentStep.value == 2) {
    isActive.value = true;
    isActive3.value = true;
    const res = await form2.value.validate();
    if (res.valid) {
      currentStep.value = 3;
    } else {
      alert("error");
    }
  }
  if (currentStep.value === 3) {
    isActive3.value = true;
    // console.log('Done: ', JSON.stringify(values, null, 2));
    return;
  }
  // currentStep.value++;
}

function prevStep() {
  if (currentStep.value <= 0) {
    return;
  }
  isActive.value = true;
  currentStep.value--;
}
</script>
<template>
  <VCard>
    <div class="formclass">
      <h1>Validate form wizard</h1>
      <p>
        This example showcases a simple multi-step form (form wizard), basically
        we use the `handleSubmit` function before moving to the next step, which
        allows us to validate the current step without having to submit the
        form.

        <br />
        <br />
        <br />
        <VRow>
          <VCol cols="6" md="2">
            <div v-if="currentStep === 0">
              <span class="box">1</span><span class="boxtext">Account ></span>
            </div>
            <div v-else>
              <span class="noboxshadow" :class="{ active: isActive }">1</span
              ><span class="noboxshadowtext">Account ></span>
            </div>
          </VCol>
          <VCol cols="6" md="2">
            <div v-if="currentStep === 1">
              <span class="box">2</span
              ><span class="boxtext">Information ></span>
            </div>
            <div v-else>
              <span class="noboxshadow" :class="{ active: isActive }">2</span
              ><span class="noboxshadowtext">Information ></span>
            </div>
          </VCol>
          <VCol cols="6" md="2">
            <div v-if="currentStep === 2">
              <span class="box">3</span><span class="boxtext">Address ></span>
            </div>
            <div v-else>
              <span class="noboxshadow" :class="{ active: isActive3 }">3</span
              ><span class="noboxshadowtext">Address ></span>
            </div>
          </VCol>
          <VCol cols="6" md="2">
            <div v-if="currentStep === 3">
              <span class="box">4</span><span class="boxtext">Price ></span>
            </div>
            <div v-else>
              <span class="noboxshadow">4</span
              ><span class="noboxshadowtext">Price ></span>
            </div>
          </VCol> </VRow
        ><br /><br />
        <VDivider></VDivider><br />
      </p>

      <Form
        @submit="nextStep"
        :validation-schema="currentSchema"
        keep-values
        Vslot="{ handleSubmit }"
      >
        <template v-if="currentStep === 0">
          <VForm ref="form" @submit.prevent lazy-validation>
            <VRow>
              <VCol cols="12" md="6">
                <VTextField
                  v-model="name"
                  placeholder="Your Name"
                  persistent-placeholder
                  :rules="[requiredValidator]"
                />
              </VCol>

              <VCol cols="12" md="6">
                <VTextField
                  v-model="email"
                  placeholder="Your Email"
                  persistent-placeholder
                  :rules="[requiredValidator, emailValidator]"
                />
              </VCol>

              <VCol cols="12" md="6">
                <VTextField
                  v-model="password"
                  type="password"
                  placeholder="Your Password"
                  persistent-placeholder
                  :rules="[requiredValidator, passwordValidator]"
                  autocomplete="on"
                />
              </VCol>

              <VCol cols="12" md="6">
                <VTextField
                  v-model="confirmPassword"
                  type="password"
                  placeholder="Confirm Password"
                  persistent-placeholder
                  :rules="[
                    requiredValidator,
                    confirmedValidator(confirmPassword, password),
                  ]"
                  autocomplete="on"
                />
              </VCol>
            </VRow>
          </VForm>
        </template>

        <template v-if="currentStep === 1">
          <VForm ref="form1" lazy-validation>
            <VRow>
              <VCol cols="12" md="6">
                <VTextField
                  v-model="company"
                  label="Company"
                  placeholder="Company"
                  :rules="[requiredValidator]"
                />
              </VCol>
              <VCol cols="12" md="6">
                <VTextField
                  v-model="city"
                  label="City"
                  placeholder="City"
                  :rules="[requiredValidator]"
                />
              </VCol>
              <VCol cols="12" md="6">
                <VTextField
                  v-model="country"
                  label="Country"
                  placeholder="Country"
                  :rules="[requiredValidator]"
                />
              </VCol>
            </VRow>
          </VForm>
        </template>

        <template v-if="currentStep === 2">
          <VForm ref="form2" lazy-validation>
            <VRow>
              <VCol cols="12" md="6">
                <VTextField
                  v-model="company1"
                  label="Company1"
                  placeholder="Company"
                  :rules="[requiredValidator]"
                />
              </VCol>
              <VCol cols="12" md="6">
                <VTextarea
                  v-model="textareaValue"
                  label="Address"
                  rows="2"
                  :rules="[requiredValidator]"
                />
              </VCol>
            </VRow>
          </VForm>
        </template>
        <VRow>
          <VCol cols="12" md="12">
            <template v-if="currentStep === 3">
              <label for="terms">Agree to terms and conditions</label>
              <Field name="terms" type="checkbox" id="terms" :value="true" />
              <ErrorMessage name="terms" /> </template
            ><br />

            <VBtn
              v-if="currentStep !== 0 && currentStep !== 3"
              type="button"
              :rounded="0"
              color="white"
              height="45px"
              class="previous"
              @click="prevStep"
            >
              &lt; Previous
            </VBtn>

            <VBtn
              start
              :rounded="0"
              color="error"
              v-if="currentStep !== 3"
              @click="nextStep"
              type="button"
              height="45px"
              class="float-right"
            >
              Next >
            </VBtn>

            <!-- <button v-if="currentStep === 3" type="button">Finish</button> -->

            <!-- <pre>{{ values }}</pre> -->
          </VCol>
        </VRow>
      </Form>
    </div>
  </VCard>
</template>
<style scoped>
.formclass {
  padding: 20px;
  margin: 20px;
}

.box {
  border: 1px solid red;
  background-color: red;
  box-shadow: 2px 2px 2px;
  margin-inline-end: 20px;
  padding-block: 8px 7px;
  padding-inline: 15px;
  text-align: center;
}

.noboxshadow {
  border: 1px solid grey;
  background-color: grey;
  margin-inline-end: 20px;
  padding-block: 8px 7px;
  padding-inline: 15px;
  text-align: center;
}

.right {
  float: inline-end;
}

.previous {
  border: 1px solid black;
}

.active {
  border: 1px solid #f7145414;
  background-color: #f7145414;
  color: red;
}

.boxtext {
  display: inline;
  color: red;
}

.noboxshadowtext {
  color: grey;
}
</style>
