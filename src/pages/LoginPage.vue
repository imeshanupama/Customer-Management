<template>
  <q-page class="flex flex-center bg-grey-2">
    <!-- Centered Card -->
    <q-card
      class="shadow-24"
      style="width: 100%; max-width: 450px; border-radius: 12px; overflow: hidden"
    >
      <!-- Header Section -->
      <div class="bg-primary text-white q-pa-lg text-center">
        <q-icon name="dashboard" size="60px" class="q-mb-sm" />
        <div class="text-h5 text-weight-bold">ClientFlow</div>
        <div class="text-caption text-blue-2 q-mt-xs">Welcome back! Please login.</div>
      </div>

      <!-- Form Section -->
      <q-card-section class="q-pa-xl">
        <q-form @submit="onSubmit" class="q-gutter-md">
          <q-input
            v-model="email"
            label="Email Address"
            outlined
            dense
            type="email"
            lazy-rules
            :rules="[(val) => (val && val.length > 0) || 'Please type your email']"
            color="primary"
            bg-color="white"
          />

          <q-input
            v-model="password"
            label="Password"
            outlined
            dense
            :type="showPassword ? 'text' : 'password'"
            lazy-rules
            :rules="[(val) => (val && val.length > 0) || 'Please type your password']"
            color="primary"
            bg-color="white"
          >
            <template v-slot:append>
              <q-icon
                :name="showPassword ? 'visibility' : 'visibility_off'"
                class="cursor-pointer"
                @click="showPassword = !showPassword"
              />
            </template>
          </q-input>

          <div class="row justify-between items-center q-mb-md">
            <q-checkbox v-model="rememberMe" label="Remember me" size="sm" color="primary" />
            <router-link
              to="#"
              class="text-primary text-weight-medium"
              style="text-decoration: none; font-size: 0.9em"
            >
              Forgot Password?
            </router-link>
          </div>

          <q-btn
            unelevated
            color="primary"
            label="Sign In"
            class="full-width q-py-sm text-weight-bold"
            size="lg"
            type="submit"
          />

          <div class="relative-position text-center q-my-md">
            <q-separator />
            <span class="bg-white q-px-sm text-grey-7 absolute-center text-caption">OR</span>
          </div>

          <q-btn
            outline
            color="grey-8"
            label="Sign in with Google"
            class="full-width"
            icon="img:https://upload.wikimedia.org/wikipedia/commons/5/53/Google_%22G%22_Logo.svg"
            no-caps
          />
        </q-form>

        <div class="text-center q-mt-lg text-grey-8">
          Don't have an account?
          <router-link
            to="/register"
            class="text-primary text-weight-bold"
            style="text-decoration: none"
          >
            Create Account
          </router-link>
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useQuasar } from 'quasar'

const router = useRouter()
const $q = useQuasar()

const email = ref('')
const password = ref('')
const rememberMe = ref(false)
const showPassword = ref(false)

const onSubmit = () => {
  $q.loading.show()

  setTimeout(() => {
    $q.loading.hide()
    $q.notify({
      type: 'positive',
      message: 'Login Successful',
      position: 'top',
    })
    router.push('/dashboard')
  }, 1000)
}
</script>

<style scoped>
.bg-grey-2 {
  background-color: #f0f2f5;
}
</style>
