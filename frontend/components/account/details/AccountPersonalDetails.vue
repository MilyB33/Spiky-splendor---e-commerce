<template>
  <v-card class="pa-4 d-flex flex-column ga-4">
    <h2>Your Data</h2>
    <form
      @submit="onSubmit"
      class="d-flex flex-column ga-2"
    >
      <div class="d-flex ga-2">
        <v-text-field
          variant="outlined"
          label="First name"
          density="compact"
          v-model="firstName"
          :error-messages="firstNameError"
        ></v-text-field>
        <v-text-field
          variant="outlined"
          label="Last name"
          density="compact"
          v-model="lastName"
          :error-messages="lastNameError"
        ></v-text-field>
      </div>
      <v-text-field
        variant="outlined"
        label="Email"
        density="compact"
        type="email"
        v-model="email"
        :error-messages="emailError"
      ></v-text-field>
      <v-text-field
        variant="outlined"
        label="Phone"
        density="compact"
        class="w-50"
        v-model="phone"
        :error-messages="phoneError"
      ></v-text-field>

      <v-btn
        type="submit"
        :disabled="isUpdatingCustomer"
        color="blue"
        style="grid-column: span 2"
        >Save</v-btn
      >
    </form>

    <div class="d-flex ga-4">
      <v-btn>Change password</v-btn>
    </div>

    <v-divider />

    <p class="text-body-2">
      Account created:
      {{
        customer?.customer
          ? new Date(customer?.customer.created_at).toLocaleDateString("pl-PL")
          : ""
      }}
    </p>
  </v-card>
</template>

<script lang="ts" setup>
import { personalDetailsTypedSchema } from "~/utils/validation/personal-details";

const { customer } = useCustomer();
const { updateCustomer, isUpdatingCustomer } = useCustomer();

const initialValues = computed(() => ({
  firstName: customer.value?.customer.first_name,
  lastName: customer.value?.customer.last_name,
  email: customer.value?.customer.email,
  phone: customer.value?.customer.phone || "",
}));

const form = useForm({
  validationSchema: personalDetailsTypedSchema,
  initialValues: initialValues.value,
});

const onSubmit = form.handleSubmit(async (values) => {
  await updateCustomer({
    first_name: values.firstName,
    last_name: values.lastName,
    email: values.email,
    phone: values.phone,
  });
});

const { value: firstName, errorMessage: firstNameError } = useField<string>("firstName");
const { value: lastName, errorMessage: lastNameError } = useField<string>("lastName");
const { value: email, errorMessage: emailError } = useField<string>("email");
const { value: phone, errorMessage: phoneError } = useField<string>("phone");
</script>
