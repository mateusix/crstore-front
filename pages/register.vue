<template>
  <body style="background-color: #fafafa; height: 100vh">
    <v-container style="height: 100vh;" class="d-flex justify-center align-center">
        <v-card class="mx-auto pa-2" color="white" height="340" width="400">
        <v-form
          v-model="form"
          @submit.prevent="onSubmit"
        >
        <v-text-field
            v-model="username"
            :readonly="loading"
            :rules="[required]"
            class="mb-2"
            label="username"
            clearable
          ></v-text-field>


          <v-text-field
            v-model="email"
            :readonly="loading"
            :rules="[required]"
            class="mb-2"
            label="Email"
            clearable
          ></v-text-field>
  
          <v-text-field
            v-model="password"
            :readonly="loading"
            :rules="[required]"
            label="Password"
            placeholder="Enter your password"
            clearable
          ></v-text-field>
  
          <br>
  
          <v-btn
            :disabled="!form"
            :loading="loading"
            color="success"
            size="large"
            type="submit"
            variant="elevated"
            block
          >
          Create account
          </v-btn>
        </v-form>
      </v-card>
  </v-container>
  </body>
</template>

<script>
definePageMeta({
    layout: 'admin'
  });
  export default {
    data: () => ({
      form: false,
      email: null,
      password: null,
      loading: false,
      username: null,
    }),

    methods: {
      onSubmit () {
        if (!this.form) return

        this.loading = true

        setTimeout(() => (this.loading = false), 2000)
      },
      required (v) {
        return !!v || 'Field is required'
      },
    },
  }
</script>