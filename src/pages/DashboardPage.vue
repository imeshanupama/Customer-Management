<template>
  <q-page class="q-pa-md bg-grey-1">
    <!-- Header -->
    <div class="row items-center justify-between q-mb-lg">
      <div>
        <h1 class="text-h4 text-weight-bold text-dark q-my-none">Dashboard</h1>
        <div class="text-grey-7">Welcome back, here's what's happening today.</div>
      </div>
      <div class="row q-gutter-sm">
        <q-btn outline color="primary" icon="cloud_download" label="Export Report" no-caps />
        <q-btn unelevated color="primary" icon="add" label="New Customer" no-caps />
      </div>
    </div>

    <!-- Stats Cards -->
    <div class="row q-col-gutter-md q-mb-xl">
      <div v-for="(stat, index) in stats" :key="index" class="col-12 col-sm-6 col-md-3">
        <q-card class="no-shadow border-light full-height">
          <q-card-section class="row items-center justify-between no-wrap">
            <div>
              <div class="text-caption text-grey-7 text-uppercase text-weight-medium">
                {{ stat.title }}
              </div>
              <div class="text-h4 text-weight-bold text-dark q-mt-xs">{{ stat.value }}</div>
              <div class="text-caption q-mt-sm">
                <span
                  :class="stat.trend > 0 ? 'text-positive' : 'text-negative'"
                  class="text-weight-bold"
                >
                  <q-icon :name="stat.trend > 0 ? 'trending_up' : 'trending_down'" />
                  {{ Math.abs(stat.trend) }}%
                </span>
                <span class="text-grey-6 q-ml-xs">vs last month</span>
              </div>
            </div>
            <q-avatar :color="stat.bg" :text-color="stat.color" size="50px" font-size="24px">
              <q-icon :name="stat.icon" />
            </q-avatar>
          </q-card-section>
        </q-card>
      </div>
    </div>

    <!-- Main Content -->
    <div class="row q-col-gutter-lg">
      <!-- Left Column: Revenue & Deals -->
      <div class="col-12 col-md-8">
        <!-- Revenue Chart Mockup -->
        <q-card class="no-shadow border-light q-mb-lg">
          <q-card-section>
            <div class="row items-center justify-between q-mb-md">
              <div class="text-h6 text-weight-bold text-dark">Revenue Overview</div>
              <q-btn-toggle
                v-model="period"
                flat
                dense
                toggle-color="primary"
                :options="[
                  { label: 'Weekly', value: 'week' },
                  { label: 'Monthly', value: 'month' },
                  { label: 'Yearly', value: 'year' },
                ]"
              />
            </div>

            <!-- CSS Bar Chart Mockup -->
            <div class="row items-end justify-between q-mt-xl q-px-sm" style="height: 250px">
              <div v-for="n in 12" :key="n" class="column items-center" style="width: 6%">
                <div
                  class="rounded-borders bg-blue-1 relative-position hover-bar"
                  :style="`width: 100%; height: ${Math.random() * 80 + 20}%`"
                >
                  <div
                    class="absolute-bottom bg-primary rounded-borders"
                    :style="`width: 100%; height: ${Math.random() * 100}%`"
                  ></div>
                </div>
                <div class="text-caption text-grey-6 q-mt-sm">Mon</div>
              </div>
            </div>
          </q-card-section>
        </q-card>

        <!-- Recent Deals Table -->
        <q-card class="no-shadow border-light">
          <q-card-section class="row items-center justify-between">
            <div class="text-h6 text-weight-bold text-dark">Recent Deals</div>
            <q-btn flat color="primary" label="View All" no-caps />
          </q-card-section>
          <q-table
            :rows="recentDeals"
            :columns="columns"
            row-key="id"
            flat
            :pagination="{ rowsPerPage: 5 }"
            hide-pagination
          >
            <template v-slot:body-cell-status="props">
              <q-td :props="props">
                <q-badge :color="getStatusColor(props.value)" class="q-py-xs q-px-sm">
                  {{ props.value }}
                </q-badge>
              </q-td>
            </template>
            <template v-slot:body-cell-value="props">
              <q-td :props="props" class="text-weight-bold">
                {{ props.value }}
              </q-td>
            </template>
          </q-table>
        </q-card>
      </div>

      <!-- Right Column: Activity & Tasks -->
      <div class="col-12 col-md-4">
        <!-- To Do List -->
        <q-card class="no-shadow border-light q-mb-lg full-height">
          <q-card-section>
            <div class="text-h6 text-weight-bold text-dark q-mb-sm">My Tasks</div>
          </q-card-section>

          <q-list separator>
            <q-item clickable v-ripple v-for="task in tasks" :key="task.id">
              <q-item-section avatar top>
                <q-checkbox v-model="task.done" />
              </q-item-section>
              <q-item-section>
                <q-item-label :class="{ 'text-strike text-grey': task.done }">{{
                  task.title
                }}</q-item-label>
                <q-item-label caption>{{ task.due }}</q-item-label>
              </q-item-section>
            </q-item>
          </q-list>
          <q-card-actions align="center">
            <q-btn flat color="primary" icon="add" label="Add New Task" class="full-width" />
          </q-card-actions>
        </q-card>

        <!-- Activity Feed -->
        <q-card class="no-shadow border-light">
          <q-card-section>
            <div class="text-h6 text-weight-bold text-dark q-mb-sm">Recent Activity</div>
          </q-card-section>
          <q-scroll-area style="height: 300px">
            <q-list class="q-pr-md">
              <q-item v-for="activity in activities" :key="activity.id">
                <q-item-section avatar>
                  <q-avatar
                    size="sm"
                    :color="activity.color"
                    text-color="white"
                    :icon="activity.icon"
                  />
                </q-item-section>
                <q-item-section>
                  <q-item-label class="text-weight-medium">{{ activity.title }}</q-item-label>
                  <q-item-label caption>{{ activity.time }}</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-scroll-area>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'

const period = ref('week')

const stats = [
  {
    title: 'Total Revenue',
    value: '$54,230',
    trend: 12.5,
    icon: 'attach_money',
    color: 'blue-9',
    bg: 'blue-1',
  },
  {
    title: 'New Leads',
    value: '1,250',
    trend: 8.2,
    icon: 'group_add',
    color: 'green-9',
    bg: 'green-1',
  },
  {
    title: 'Active Deals',
    value: '45',
    trend: -2.4,
    icon: 'handshake',
    color: 'orange-9',
    bg: 'orange-1',
  },
  {
    title: 'Pending Tickets',
    value: '12',
    trend: -5.0,
    icon: 'confirmation_number',
    color: 'purple-9',
    bg: 'purple-1',
  },
]

const recentDeals = [
  {
    id: 1,
    customer: 'Acme Corp',
    amount: '$12,500',
    stage: 'Negotiation',
    status: 'In Progress',
    owner: 'John D.',
  },
  {
    id: 2,
    customer: 'GlobalTech',
    amount: '$5,000',
    stage: 'Discovery',
    status: 'New',
    owner: 'Sarah M.',
  },
  {
    id: 3,
    customer: 'Stark Ind',
    amount: '$85,000',
    stage: 'Contract',
    status: 'Closing',
    owner: 'Tony S.',
  },
  {
    id: 4,
    customer: 'Wayne Ent',
    amount: '$42,000',
    stage: 'Proposal',
    status: 'Review',
    owner: 'Bruce W.',
  },
  {
    id: 5,
    customer: 'Cyberdyne',
    amount: '$150,000',
    stage: 'Qualified',
    status: 'On Hold',
    owner: 'John C.',
  },
]

const columns = [
  { name: 'customer', label: 'Customer', field: 'customer', align: 'left' },
  { name: 'amount', label: 'Amount', field: 'amount', align: 'left' },
  { name: 'stage', label: 'Stage', field: 'stage', align: 'left' },
  { name: 'status', label: 'Status', field: 'status', align: 'left' },
  { name: 'owner', label: 'Owner', field: 'owner', align: 'left' },
]

const tasks = ref([
  { id: 1, title: 'Follow up with GlobalTech', due: 'Today, 2:00 PM', done: false },
  { id: 2, title: 'Send contract to Stark Ind', due: 'Tomorrow, 10:00 AM', done: false },
  { id: 3, title: 'Review Q3 Marketing Report', due: 'Sep 24, 2024', done: true },
  { id: 4, title: 'Update CRM records', due: 'Sep 25, 2024', done: false },
])

const activities = [
  {
    id: 1,
    title: 'New lead "Tesla Inc" added',
    time: '2 mins ago',
    icon: 'person_add',
    color: 'blue',
  },
  { id: 2, title: 'Email sent to Acme Corp', time: '1 hour ago', icon: 'email', color: 'grey' },
  {
    id: 3,
    title: 'Deal won: Stark Industries',
    time: '4 hours ago',
    icon: 'emoji_events',
    color: 'amber',
  },
  {
    id: 4,
    title: 'Meeting scheduled with Wayne Ent',
    time: 'Yesterday',
    icon: 'event',
    color: 'purple',
  },
  {
    id: 5,
    title: 'Ticket #402 resolved',
    time: '2 days ago',
    icon: 'check_circle',
    color: 'green',
  },
]

const getStatusColor = (status) => {
  switch (status) {
    case 'In Progress':
      return 'blue-2 text-blue-9'
    case 'New':
      return 'green-2 text-green-9'
    case 'Closing':
      return 'purple-2 text-purple-9'
    case 'Review':
      return 'orange-2 text-orange-9'
    case 'On Hold':
      return 'grey-3 text-grey-8'
    default:
      return 'grey'
  }
}
</script>

<style lang="scss" scoped>
.border-light {
  border: 1px solid #edf2f7;
}
.hover-bar {
  transition: all 0.3s ease;
  cursor: pointer;
}
.hover-bar:hover {
  transform: scaleY(1.05);
  opacity: 0.9;
}
</style>
