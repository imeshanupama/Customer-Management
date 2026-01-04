<template>
  <q-layout view="lHh Lpr lFf" class="bg-grey-1">
    <q-header elevated class="bg-white text-dark q-py-xs" height-hint="58">
      <q-toolbar>
        <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />

        <q-btn flat no-caps no-wrap class="q-ml-xs" to="/">
          <q-icon name="dashboard" color="primary" size="28px" />
          <q-toolbar-title shrink class="text-weight-bold">
            Client<span class="text-primary">Flow</span>
          </q-toolbar-title>
        </q-btn>

        <q-space />

        <!-- App Header: Search & Actions (Visible only inside App) -->
        <template v-if="!isLanding">
          <!-- Global Search Bar -->
          <div class="col-auto gt-sm" style="width: 400px">
            <q-input
              dense
              outlined
              rounded
              v-model="searchQuery"
              placeholder="Search customers, deals, pages..."
              bg-color="grey-1"
              class="full-width"
              @update:model-value="performSearch"
              @focus="showResults = true"
            >
              <template v-slot:prepend>
                <q-icon name="search" class="text-grey-6" />
              </template>

              <!-- Search Results Dropdown -->
              <q-menu
                fit
                v-model="showResults"
                no-parent-event
                :offset="[0, 10]"
                style="min-height: 150px"
              >
                <div v-if="!searchQuery" class="q-pa-md text-center text-grey-7">
                  <q-icon name="manage_search" size="40px" color="grey-4" />
                  <div class="q-mt-sm">Type to search across ClientFlow</div>
                </div>

                <q-list v-else style="min-width: 350px">
                  <!-- Pages Group -->
                  <div v-if="filteredResults.pages.length > 0">
                    <q-item-label header class="text-uppercase text-caption text-weight-bold"
                      >Pages</q-item-label
                    >
                    <q-item
                      v-for="page in filteredResults.pages"
                      :key="page.title"
                      clickable
                      v-close-popup
                      :to="page.link"
                      class="items-center"
                    >
                      <q-item-section avatar>
                        <q-avatar icon="link" color="grey-3" text-color="dark" size="sm" />
                      </q-item-section>
                      <q-item-section>{{ page.title }}</q-item-section>
                      <q-item-section side>
                        <q-icon name="arrow_forward" size="xs" />
                      </q-item-section>
                    </q-item>
                  </div>

                  <!-- Customers Group -->
                  <div v-if="filteredResults.customers.length > 0">
                    <q-separator spaced />
                    <q-item-label header class="text-uppercase text-caption text-weight-bold"
                      >Customers</q-item-label
                    >
                    <q-item
                      v-for="customer in filteredResults.customers"
                      :key="customer.id"
                      clickable
                      v-close-popup
                      :to="`/customers/${customer.id}`"
                    >
                      <q-item-section avatar>
                        <q-avatar :color="customer.avatarColor" text-color="white" size="sm">
                          {{ customer.name.charAt(0) }}
                        </q-avatar>
                      </q-item-section>
                      <q-item-section>
                        <q-item-label>{{ customer.name }}</q-item-label>
                        <q-item-label caption>{{ customer.company }}</q-item-label>
                      </q-item-section>
                    </q-item>
                  </div>

                  <div
                    v-if="
                      filteredResults.pages.length === 0 && filteredResults.customers.length === 0
                    "
                    class="q-pa-md text-center text-grey"
                  >
                    No results found for "{{ searchQuery }}"
                  </div>
                </q-list>
              </q-menu>
            </q-input>
          </div>

          <q-space />

          <!-- App Actions -->
          <div class="row q-gutter-md items-center action-buttons">
            <!-- Notifications -->
            <q-btn round dense flat color="grey-8" icon="notifications">
              <q-badge color="red" floating rounded>3</q-badge>
              <q-menu>
                <q-list style="min-width: 320px">
                  <q-item-label header class="text-weight-bold">Notifications</q-item-label>

                  <q-item clickable v-ripple class="q-py-md">
                    <q-item-section avatar>
                      <q-avatar icon="person_add" color="blue-1" text-color="primary" />
                    </q-item-section>
                    <q-item-section>
                      <q-item-label>New Lead Assigned</q-item-label>
                      <q-item-label caption
                        >John Doe from Acme Corp was assigned to you.</q-item-label
                      >
                    </q-item-section>
                    <q-item-section side top>
                      <q-item-label caption>2m</q-item-label>
                      <q-icon name="circle" color="primary" size="10px" />
                    </q-item-section>
                  </q-item>

                  <q-separator inset="item" />

                  <q-item clickable v-ripple class="q-py-md">
                    <q-item-section avatar>
                      <q-avatar icon="payments" color="green-1" text-color="green" />
                    </q-item-section>
                    <q-item-section>
                      <q-item-label>Deal Won!</q-item-label>
                      <q-item-label caption>GlobalTech deal was marked as Won.</q-item-label>
                    </q-item-section>
                    <q-item-section side top>
                      <q-item-label caption>1h</q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-separator inset="item" />

                  <q-item clickable v-ripple class="q-py-md">
                    <q-item-section avatar>
                      <q-avatar icon="warning" color="orange-1" text-color="orange" />
                    </q-item-section>
                    <q-item-section>
                      <q-item-label>Task Overdue</q-item-label>
                      <q-item-label caption>Follow up call with Microsoft is overdue.</q-item-label>
                    </q-item-section>
                    <q-item-section side top>
                      <q-item-label caption>5h</q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-separator />
                  <q-item clickable class="text-center text-primary text-weight-bold">
                    <q-item-section>View All</q-item-section>
                  </q-item>
                </q-list>
              </q-menu>
            </q-btn>

            <!-- User Profile -->
            <q-btn flat no-wrap no-caps class="q-ml-sm">
              <q-avatar size="32px">
                <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
              </q-avatar>
              <q-icon name="arrow_drop_down" size="16px" />

              <q-menu auto-close>
                <q-list style="min-width: 150px">
                  <q-item clickable to="/settings">
                    <q-item-section avatar><q-icon name="person" /></q-item-section>
                    <q-item-section>Profile</q-item-section>
                  </q-item>
                  <q-item clickable to="/settings">
                    <q-item-section avatar><q-icon name="settings" /></q-item-section>
                    <q-item-section>Settings</q-item-section>
                  </q-item>
                  <q-separator />
                  <q-item clickable to="/">
                    <q-item-section avatar><q-icon name="logout" /></q-item-section>
                    <q-item-section>Log Out</q-item-section>
                  </q-item>
                </q-list>
              </q-menu>
            </q-btn>
          </div>
        </template>

        <!-- Landing Page Header (Visible only on Landing) -->
        <template v-else>
          <div class="q-gutter-sm row items-center no-wrap gt-sm">
            <q-btn flat label="Features" />
            <q-btn flat label="Pricing" />
            <q-btn flat label="Support" />
            <q-btn unelevated color="primary" label="Log In" to="/login" class="q-px-md" />
          </div>
        </template>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered class="bg-white" :width="280">
      <div class="column full-height">
        <q-scroll-area class="col">
          <q-list padding class="text-grey-8">
            <q-item-label
              header
              class="text-weight-bold text-uppercase text-grey-6 text-subtitle2 q-pt-md"
            >
              Navigation
            </q-item-label>

            <EssentialLink v-for="link in linksList" :key="link.title" v-bind="link" />

            <q-separator class="q-my-md" />

            <q-item-label header class="text-weight-bold text-uppercase text-grey-6 text-subtitle2">
              Resources
            </q-item-label>

            <q-item clickable v-ripple href="#">
              <q-item-section avatar>
                <q-icon name="help" />
              </q-item-section>
              <q-item-section>
                <q-item-label>Help Center</q-item-label>
                <q-item-label caption>Guides and FAQs</q-item-label>
              </q-item-section>
            </q-item>
          </q-list>
        </q-scroll-area>

        <div class="q-pa-md bg-grey-2">
          <div class="text-caption text-grey-7 text-center">
            &copy; {{ new Date().getFullYear() }} ClientFlow. <br />
            All rights reserved.
          </div>
        </div>
      </div>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
import { useRoute } from 'vue-router'
import EssentialLink from 'components/EssentialLink.vue'

const route = useRoute()
const isLanding = computed(() => route.path === '/')

// Search State
const searchQuery = ref('')
const showResults = ref(false)
const filteredResults = reactive({
  pages: [],
  customers: [],
})

// Mock Data for Search
const searchData = {
  pages: [
    { title: 'Dashboard', link: '/dashboard' },
    { title: 'Customer Management', link: '/customers' },
    { title: 'Lead Tracking', link: '/deals' },
    { title: 'Analytics Reports', link: '/reports' },
    { title: 'Settings', link: '/settings' },
  ],
  customers: [
    { id: 1, name: 'John Doe', company: 'Acme Corp', avatarColor: 'blue' },
    { id: 2, name: 'Sarah Smith', company: 'GlobalTech', avatarColor: 'green' },
    { id: 3, name: 'Michael Brown', company: 'Nebula.io', avatarColor: 'purple' },
    { id: 4, name: 'Emily Davis', company: 'FoxRun', avatarColor: 'orange' },
  ],
}

function performSearch(val) {
  if (!val) {
    filteredResults.pages = []
    filteredResults.customers = []
    return
  }

  const query = val.toLowerCase()

  filteredResults.pages = searchData.pages.filter((p) => p.title.toLowerCase().includes(query))
  filteredResults.customers = searchData.customers.filter(
    (c) => c.name.toLowerCase().includes(query) || c.company.toLowerCase().includes(query),
  )
}

const linksList = [
  {
    title: 'Dashboard',
    caption: 'Overview & Stats',
    icon: 'space_dashboard',
    link: '/dashboard', // Updated to point to new Dashboard page
  },
  {
    title: 'Customer Management',
    caption: 'Profiles & History',
    icon: 'people',
    link: '/customers',
  },
  {
    title: 'Lead Tracking',
    caption: 'Pipelines & Opportunities',
    icon: 'trending_up',
    link: '/deals',
  },
  {
    title: 'Communication',
    caption: 'Email & Tickets',
    icon: 'chat',
    link: '#',
  },
  {
    title: 'Analytics',
    caption: 'Reports & Insights',
    icon: 'analytics',
    link: '/reports',
  },
  {
    title: 'Settings',
    caption: 'Preferences',
    icon: 'settings',
    link: '/settings',
  },
]

const leftDrawerOpen = ref(false)

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
</script>
