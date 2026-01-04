<template>
  <q-page class="q-pa-md bg-grey-1">
    <div class="row items-center justify-between q-mb-lg">
      <h1 class="text-h4 text-weight-bold text-dark q-my-none">Settings</h1>
    </div>

    <div class="row q-col-gutter-lg">
      <!-- Left Column: Settings Navigation/Tabs -->
      <div class="col-12 col-md-3">
        <q-card class="no-shadow border-light">
          <q-tabs
            v-model="tab"
            vertical
            class="text-grey-7"
            active-color="primary"
            active-bg-color="blue-1"
            indicator-color="primary"
            align="justify"
          >
            <q-tab name="profile" label="Profile" icon="person" class="justify-start q-px-lg" />
            <q-tab
              name="appearance"
              label="Appearance"
              icon="dark_mode"
              class="justify-start q-px-lg"
            />
            <q-tab
              name="notifications"
              label="Notifications"
              icon="notifications"
              class="justify-start q-px-lg"
            />
            <q-tab name="security" label="Security" icon="lock" class="justify-start q-px-lg" />
          </q-tabs>
        </q-card>
      </div>

      <!-- Right Column: Content -->
      <div class="col-12 col-md-9">
        <q-tab-panels v-model="tab" animated class="bg-transparent">
          <!-- Profile Settings -->
          <q-tab-panel name="profile" class="q-pa-none">
            <q-card class="no-shadow border-light">
              <q-card-section>
                <div class="text-h6 q-mb-md">Public Profile</div>
                <div class="row q-col-gutter-md items-center q-mb-lg">
                  <div class="col-12 col-sm-auto">
                    <q-avatar size="100px" class="bg-grey-3">
                      <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
                    </q-avatar>
                  </div>
                  <div class="col-12 col-sm-auto">
                    <q-btn outline color="primary" label="Change Avatar" size="sm" />
                    <q-btn flat color="negative" label="Remove" size="sm" class="q-ml-sm" />
                  </div>
                </div>

                <q-form class="q-gutter-md">
                  <div class="row q-col-gutter-md">
                    <div class="col-12 col-md-6">
                      <q-input outlined v-model="profile.firstName" label="First Name" />
                    </div>
                    <div class="col-12 col-md-6">
                      <q-input outlined v-model="profile.lastName" label="Last Name" />
                    </div>
                  </div>
                  <q-input outlined v-model="profile.email" label="Email" />
                  <q-input outlined v-model="profile.bio" type="textarea" label="Bio" rows="3" />

                  <div class="row justify-end">
                    <q-btn color="primary" label="Save Changes" unelevated />
                  </div>
                </q-form>
              </q-card-section>
            </q-card>
          </q-tab-panel>

          <!-- Appearance Settings -->
          <q-tab-panel name="appearance" class="q-pa-none">
            <q-card class="no-shadow border-light">
              <q-card-section>
                <div class="text-h6 q-mb-md">Theme Preferences</div>

                <q-list padding>
                  <q-item tag="label" v-ripple>
                    <q-item-section>
                      <q-item-label>Dark Mode</q-item-label>
                      <q-item-label caption
                        >Switch to a dark theme for low-light environments</q-item-label
                      >
                    </q-item-section>
                    <q-item-section side top>
                      <q-toggle
                        v-model="darkMode"
                        @update:model-value="toggleDarkMode"
                        color="primary"
                      />
                    </q-item-section>
                  </q-item>

                  <q-separator spaced />

                  <q-item>
                    <q-item-section>
                      <q-item-label>Compact Mode</q-item-label>
                      <q-item-label caption
                        >Reduce padding and font sizes for higher density</q-item-label
                      >
                    </q-item-section>
                    <q-item-section side top>
                      <q-toggle v-model="compactMode" color="primary" />
                    </q-item-section>
                  </q-item>
                </q-list>
              </q-card-section>
            </q-card>
          </q-tab-panel>

          <!-- Notifications Settings -->
          <q-tab-panel name="notifications" class="q-pa-none">
            <q-card class="no-shadow border-light">
              <q-card-section>
                <div class="text-h6 q-mb-md">Email Notifications</div>
                <q-list>
                  <q-item tag="label" v-ripple>
                    <q-item-section>
                      <q-item-label>New Deals</q-item-label>
                      <q-item-label caption
                        >Get notified when a new deal is assigned to you</q-item-label
                      >
                    </q-item-section>
                    <q-item-section side>
                      <q-checkbox v-model="notifications.deals" />
                    </q-item-section>
                  </q-item>
                  <q-item tag="label" v-ripple>
                    <q-item-section>
                      <q-item-label>Customer Updates</q-item-label>
                    </q-item-section>
                    <q-item-section side>
                      <q-checkbox v-model="notifications.customers" />
                    </q-item-section>
                  </q-item>
                  <q-item tag="label" v-ripple>
                    <q-item-section>
                      <q-item-label>Weekly Digest</q-item-label>
                    </q-item-section>
                    <q-item-section side>
                      <q-checkbox v-model="notifications.digest" />
                    </q-item-section>
                  </q-item>
                </q-list>
              </q-card-section>
            </q-card>
          </q-tab-panel>

          <!-- Security Settings -->
          <q-tab-panel name="security" class="q-pa-none">
            <q-card class="no-shadow border-light">
              <q-card-section>
                <div class="text-h6 q-mb-md">Password & Auth</div>
                <q-btn outline color="primary" label="Change Password" class="q-mr-sm" />
                <q-btn outline color="grey-8" label="Two-Factor Authentication" />
              </q-card-section>
            </q-card>
          </q-tab-panel>
        </q-tab-panels>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useQuasar } from 'quasar'

const $q = useQuasar()
const tab = ref('profile')

// Profile Data
const profile = reactive({
  firstName: 'John',
  lastName: 'Doe',
  email: 'john.doe@clientflow.com',
  bio: 'Sales Manager at ClientFlow. Passionate about growth and customer success.',
})

// Appearance Data
const darkMode = ref($q.dark.isActive)
const compactMode = ref(false)

const toggleDarkMode = (val) => {
  $q.dark.set(val)
}

// Notification Data
const notifications = reactive({
  deals: true,
  customers: false,
  digest: true,
})
</script>

<style lang="scss" scoped>
.border-light {
  border: 1px solid #edf2f7;
}
</style>
