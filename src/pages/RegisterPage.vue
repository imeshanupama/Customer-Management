<template>
  <q-page class="row full-screen">
    <!-- Left Side - Image/Branding -->
    <div class="col-12 col-md-6 bg-dark flex flex-center text-white p-relative gt-sm">
      <div
        class="absolute-full"
        style="background: linear-gradient(135deg, #0f172a 0%, #334155 100%); opacity: 0.9 z-index: 1;"
      ></div>
      <!-- You can replace this with a real image if you have one, or keep the gradient -->

      <div class="text-center q-pa-xl relative-position" style="z-index: 2">
        <q-icon name="rocket_launch" size="100px" color="secondary" class="q-mb-md" />
        <h2 class="text-h3 text-weight-bold q-my-sm">Join ClientFlow</h2>
        <div class="text-h5 text-grey-4 q-mb-xl">Start growing your business today.</div>

        <q-list class="text-left text-h6 q-gutter-md">
          <q-item>
            <q-item-section avatar><q-icon name="check_circle" color="secondary" /></q-item-section>
            <q-item-section>14-day free trial</q-item-section>
          </q-item>
          <q-item>
            <q-item-section avatar><q-icon name="check_circle" color="secondary" /></q-item-section>
            <q-item-section>No credit card required</q-item-section>
          </q-item>
          <q-item>
            <q-item-section avatar><q-icon name="check_circle" color="secondary" /></q-item-section>
            <q-item-section>Unlimited support</q-item-section>
          </q-item>
        </q-list>
      </div>
    </div>

    <!-- Right Side - Register Form -->
    <div class="col-12 col-md-6 flex flex-center bg-white">
      <div class="full-width q-pa-xl" style="max-width: 500px">
        <div class="text-h4 text-weight-bold text-dark q-mb-sm">Create Account 🚀</div>
        <div class="text-grey-7 q-mb-xl">Get started with your free account.</div>

        <q-form @submit="onSubmit" class="q-gutter-md">
          <div class="row q-col-gutter-sm">
            <div class="col-12 col-sm-6">
              <q-input
                outlined
                v-model="form.firstName"
                label="First Name"
                lazy-rules
                :rules="[(val) => !!val || 'Required']"
              />
            </div>
            <div class="col-12 col-sm-6">
              <q-input
                outlined
                v-model="form.lastName"
                label="Last Name"
                lazy-rules
                :rules="[(val) => !!val || 'Required']"
              />
            </div>
          </div>

          <q-input
            v-model="form.email"
            label="Email Address"
            outlined
            type="email"
            lazy-rules
            :rules="[(val) => !!val || 'Required']"
          />

          <q-input
            v-model="form.password"
            label="Password"
            outlined
            :type="showPassword ? 'text' : 'password'"
            lazy-rules
            :rules="[(val) => (val && val.length >= 8) || 'Min 8 characters']"
          >
            <template v-slot:append>
              <q-icon
                :name="showPassword ? 'visibility' : 'visibility_off'"
                class="cursor-pointer"
                @click="showPassword = !showPassword"
              />
            </template>
          </q-input>

          <q-input v-model="form.company" label="Company Name" outlined />

          <q-checkbox
            v-model="form.agree"
            label="I agree to the Terms and Privacy Policy"
            size="sm"
            color="primary"
          />

          <q-btn
            unelevated
            color="primary"
            label="Create Account"
            class="full-width q-py-sm text-weight-bold"
            size="lg"
            type="submit"
            :disable="!form.agree"
          />
        </q-form>

        <div class="text-center q-mt-xl text-grey-8">
          Already have an account?
          <router-link
            to="/login"
            class="text-primary text-weight-bold"
            style="text-decoration: none"
            >Sign In</router-link
          >
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useRouter } from 'vue-router'
import { useQuasar } from 'quasar'

const router = useRouter()
const $q = useQuasar()

const showPassword = ref(false)
const form = reactive({
  firstName: '',
  lastName: '',
  email: '',
  password: '',
  company: '',
  agree: false,
})

const onSubmit = () => {
  $q.loading.show()

  setTimeout(() => {
    $q.loading.hide()
    $q.notify({
      type: 'positive',
      message: 'Account Created!',
      position: 'top',
    })
    router.push('/dashboard')
  }, 1500)
}
</script>

<style lang="scss" scoped>
// nothing needed specifically
</style>
