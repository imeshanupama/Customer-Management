<template>
  <q-page class="q-pa-md bg-grey-1">
    <!-- Header / Overview Card -->
    <q-card class="no-shadow border-light q-mb-lg">
      <q-card-section class="q-pa-lg">
        <div class="row items-center q-col-gutter-md">
          <div class="col-12 col-sm-auto">
            <q-avatar
              size="100px"
              :color="customer.avatarColor"
              text-color="white"
              class="text-h3 text-weight-bold"
            >
              {{ customer.name.charAt(0) }}
            </q-avatar>
          </div>
          <div class="col-12 col-sm">
            <div class="row items-center q-gutter-sm q-mb-xs">
              <h1 class="text-h4 text-weight-bold text-dark q-my-none">{{ customer.name }}</h1>
              <q-badge
                :color="getStatusColor(customer.status).bg"
                :text-color="getStatusColor(customer.status).text"
                class="text-weight-bold q-px-sm q-py-xs text-uppercase"
              >
                {{ customer.status }}
              </q-badge>
            </div>
            <div class="text-h6 text-grey-7 q-mb-sm">{{ customer.company }}</div>
            <div class="row q-gutter-md text-grey-7">
              <div class="row items-center">
                <q-icon name="email" class="q-mr-xs" size="xs" /> {{ customer.email }}
              </div>
              <div class="row items-center">
                <q-icon name="phone" class="q-mr-xs" size="xs" /> {{ customer.phone }}
              </div>
              <div class="row items-center">
                <q-icon name="location_on" class="q-mr-xs" size="xs" /> {{ customer.location }}
              </div>
            </div>
          </div>
          <div class="col-12 col-sm-auto row q-gutter-sm">
            <q-btn outline color="primary" icon="email" label="Send Email" no-caps />
            <q-btn outline color="secondary" icon="call" label="Log Call" no-caps />
            <q-btn unelevated color="primary" label="Edit Profile" no-caps />
          </div>
        </div>
      </q-card-section>
    </q-card>

    <div class="row q-col-gutter-lg">
      <!-- Left Sidebar: Info -->
      <div class="col-12 col-md-4">
        <q-card class="no-shadow border-light q-mb-md">
          <q-card-section>
            <div class="text-subtitle1 text-weight-bold q-mb-md">About</div>
            <div class="text-body2 text-grey-8 q-mb-md">
              {{ customer.bio }}
            </div>

            <q-separator class="q-my-md" />

            <div class="text-subtitle2 text-weight-bold q-mb-sm">Tags</div>
            <div class="row q-gutter-sm">
              <q-chip
                v-for="tag in customer.tags"
                :key="tag"
                dense
                color="blue-1"
                text-color="blue-9"
                >{{ tag }}</q-chip
              >
              <q-btn flat round icon="add" size="sm" color="grey" />
            </div>

            <q-separator class="q-my-md" />

            <div class="text-subtitle2 text-weight-bold q-mb-sm">Details</div>
            <q-list dense>
              <q-item class="q-px-none">
                <q-item-section class="text-grey-6">Source</q-item-section>
                <q-item-section side class="text-dark">LinkedIn</q-item-section>
              </q-item>
              <q-item class="q-px-none">
                <q-item-section class="text-grey-6">Owner</q-item-section>
                <q-item-section side class="text-dark">Me</q-item-section>
              </q-item>
              <q-item class="q-px-none">
                <q-item-section class="text-grey-6">Industry</q-item-section>
                <q-item-section side class="text-dark">Software</q-item-section>
              </q-item>
            </q-list>
          </q-card-section>
        </q-card>

        <q-card class="no-shadow border-light">
          <q-card-section>
            <div class="text-subtitle1 text-weight-bold q-mb-md">Recent Deals</div>
            <q-list separator>
              <q-item v-for="deal in customer.deals" :key="deal.id" class="q-px-none">
                <q-item-section>
                  <q-item-label class="text-weight-bold">{{ deal.title }}</q-item-label>
                  <q-item-label caption>{{ deal.date }}</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <div class="text-primary text-weight-bold">{{ deal.amount }}</div>
                  <q-badge :color="deal.statusColor" class="q-mt-xs">{{ deal.status }}</q-badge>
                </q-item-section>
              </q-item>
            </q-list>
            <q-btn flat color="primary" label="View All Deals" class="full-width q-mt-sm" no-caps />
          </q-card-section>
        </q-card>
      </div>

      <!-- Right Content: Timeline & Notes -->
      <div class="col-12 col-md-8">
        <q-card class="no-shadow border-light full-height">
          <q-tabs
            v-model="tab"
            dense
            class="text-grey"
            active-color="primary"
            indicator-color="primary"
            align="left"
            narrow-indicator
          >
            <q-tab name="timeline" label="Activity Timeline" />
            <q-tab name="notes" label="Notes" />
            <q-tab name="files" label="Files" />
          </q-tabs>

          <q-separator />

          <q-tab-panels v-model="tab" animated>
            <q-tab-panel name="timeline">
              <q-timeline color="secondary">
                <q-timeline-entry
                  v-for="event in timeline"
                  :key="event.id"
                  :title="event.title"
                  :subtitle="event.date"
                  :icon="event.icon"
                  :color="event.color"
                >
                  <div class="text-grey-8">
                    {{ event.desc }}
                  </div>
                </q-timeline-entry>
              </q-timeline>
            </q-tab-panel>

            <q-tab-panel name="notes">
              <div class="q-mb-md">
                <q-input
                  outlined
                  type="textarea"
                  placeholder="Type a note..."
                  rows="3"
                  v-model="newNote"
                />
                <div class="row justify-end q-mt-sm">
                  <q-btn color="primary" label="Add Note" unelevated size="sm" />
                </div>
              </div>

              <q-list bordered class="rounded-borders">
                <q-item v-for="note in notes" :key="note.id" class="q-py-md">
                  <q-item-section avatar top>
                    <q-avatar icon="sticky_note_2" color="yellow-2" text-color="orange-9" />
                  </q-item-section>
                  <q-item-section>
                    <q-item-label class="text-weight-bold">{{ note.author }}</q-item-label>
                    <q-item-label caption class="q-mb-sm">{{ note.date }}</q-item-label>
                    <div class="text-grey-9 text-body2">{{ note.content }}</div>
                  </q-item-section>
                </q-item>
              </q-list>
            </q-tab-panel>

            <q-tab-panel name="files">
              <div class="text-center text-grey q-py-xl border-dashed">
                <q-icon name="cloud_upload" size="40px" />
                <div class="q-mt-sm">Drag and drop files here</div>
              </div>
            </q-tab-panel>
          </q-tab-panels>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const tab = ref('timeline')
const newNote = ref('')

// Mock Data (In a real app, fetch based on route.params.id)
const customer = ref({
  id: route.params.id || 1,
  name: 'John Doe',
  company: 'Acme Corp',
  email: 'john@acme.com',
  phone: '(555) 123-4567',
  location: 'New York, USA',
  status: 'Active',
  avatarColor: 'blue-8',
  bio: 'Key decision maker at Acme Corp. Interested in scaling their sales team operations using our enterprise plan.',
  tags: ['VIP', 'Decision Maker', 'SaaS'],
  deals: [
    {
      id: 1,
      title: 'Enterprise License',
      amount: '$12,500',
      date: 'Sep 20, 2024',
      status: 'In Progress',
      statusColor: 'blue',
    },
    {
      id: 2,
      title: 'Consulting Hour',
      amount: '$500',
      date: 'Aug 15, 2024',
      status: 'Won',
      statusColor: 'green',
    },
  ],
})

const timeline = [
  {
    id: 1,
    title: 'Email Sent',
    date: 'Today, 10:30 AM',
    icon: 'email',
    color: 'blue',
    desc: 'Sent proposal breakdown for the Enterprise plan.',
  },
  {
    id: 2,
    title: 'Call Logged',
    date: 'Yesterday, 4:15 PM',
    icon: 'call',
    color: 'purple',
    desc: 'Discussed pricing options. Client seems positive but needs approval from CEO.',
  },
  {
    id: 3,
    title: 'Deal Created',
    date: 'Sep 20, 2024',
    icon: 'handshake',
    color: 'green',
    desc: 'Opportunity "Enterprise License" was created worth $12,500.',
  },
  {
    id: 4,
    title: 'New Note',
    date: 'Sep 15, 2024',
    icon: 'edit',
    color: 'orange',
    desc: 'Met at TechSummit conference. Very impressed with the dashboard demo.',
  },
]

const notes = [
  {
    id: 1,
    author: 'Me',
    date: '2 hours ago',
    content: 'Remember to follow up next Tuesday regarding the contract adjustments.',
  },
  {
    id: 2,
    author: 'Sarah Smith',
    date: '3 days ago',
    content:
      'John mentioned they are evaluating competitors as well. We need to highlight our support features.',
  },
]

const getStatusColor = (status) => {
  switch (status) {
    case 'Active':
      return { bg: 'green-1', text: 'green-9' }
    case 'Lead':
      return { bg: 'blue-1', text: 'blue-9' }
    case 'Inactive':
      return { bg: 'grey-2', text: 'grey-8' }
    default:
      return { bg: 'grey-1', text: 'dark' }
  }
}
</script>

<style lang="scss" scoped>
.border-light {
  border: 1px solid #edf2f7;
}
.border-dashed {
  border: 2px dashed #e0e0e0;
  border-radius: 8px;
}
</style>
