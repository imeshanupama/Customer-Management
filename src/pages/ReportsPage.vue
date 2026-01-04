<template>
  <q-page class="q-pa-md bg-grey-1">
    <!-- Header -->
    <div class="row items-center justify-between q-mb-lg">
      <div>
        <h1 class="text-h4 text-weight-bold text-dark q-my-none">Analytics</h1>
        <div class="text-grey-7">Detailed reports and insights into your business performance.</div>
      </div>
      <div class="row q-gutter-sm">
        <q-btn outline color="grey-8" label="Last 30 Days" icon-right="arrow_drop_down" no-caps />
        <q-btn unelevated color="primary" icon="download" label="Export Report" no-caps />
      </div>
    </div>

    <!-- KPI Cards -->
    <div class="row q-col-gutter-md q-mb-lg">
      <div class="col-12 col-sm-6 col-md-3" v-for="kpi in kpis" :key="kpi.label">
        <q-card class="no-shadow border-light full-height">
          <q-card-section>
            <div class="text-grey-7 text-caption text-uppercase text-weight-bold q-mb-sm">
              {{ kpi.label }}
            </div>
            <div class="row items-center justify-between">
              <div class="text-h4 text-weight-bold text-dark">{{ kpi.value }}</div>
              <q-badge
                :color="kpi.trendUp ? 'green-1' : 'red-1'"
                :text-color="kpi.trendUp ? 'green-9' : 'red-9'"
                class="q-pa-xs"
              >
                <q-icon
                  :name="kpi.trendUp ? 'trending_up' : 'trending_down'"
                  size="14px"
                  class="q-mr-xs"
                />
                {{ kpi.trend }}
              </q-badge>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>

    <!-- Charts Section -->
    <div class="row q-col-gutter-lg">
      <!-- Revenue Chart (CSS Bar Chart Implementation) -->
      <div class="col-12 col-md-8">
        <q-card class="no-shadow border-light">
          <q-card-section>
            <div class="row items-center justify-between q-mb-lg">
              <div class="text-h6 text-weight-bold">Revenue Overview</div>
              <div class="row q-gutter-xs">
                <q-btn flat dense size="sm" color="primary" label="12 Months" />
                <q-btn flat dense size="sm" color="grey" label="30 Days" />
                <q-btn flat dense size="sm" color="grey" label="7 Days" />
              </div>
            </div>

            <!-- Chart Container -->
            <div
              class="row items-end justify-between q-mt-xl"
              style="height: 250px; padding: 0 10px"
            >
              <div
                v-for="(bar, index) in chartData"
                :key="index"
                class="column items-center slide-up"
                style="width: 8%"
              >
                <q-tooltip>{{ bar.label }}: {{ bar.amount }}</q-tooltip>
                <div
                  class="bg-primary rounded-borders q-mb-sm transition-height"
                  :style="{ height: bar.height + '%', width: '100%', opacity: 0.8 }"
                ></div>
                <div class="text-caption text-grey-6">{{ bar.label }}</div>
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>

      <!-- Lead Sources (Progress Bars) -->
      <div class="col-12 col-md-4">
        <q-card class="no-shadow border-light full-height">
          <q-card-section>
            <div class="text-h6 text-weight-bold q-mb-lg">Lead Sources</div>

            <div class="q-gutter-y-lg">
              <div v-for="source in sources" :key="source.name">
                <div class="row justify-between q-mb-xs">
                  <div class="text-weight-medium">{{ source.name }}</div>
                  <div class="text-grey-7">{{ source.value }}%</div>
                </div>
                <q-linear-progress
                  :value="source.value / 100"
                  rounded
                  size="10px"
                  :color="source.color"
                  track-color="grey-2"
                />
              </div>
            </div>

            <div class="q-mt-xl bg-grey-1 q-pa-md rounded-borders">
              <div class="text-subtitle2 text-weight-bold q-mb-sm">Analyst Note 💡</div>
              <div class="text-caption text-grey-8">
                LinkedIn campaigns are converting 20% higher than last month. Consider increasing ad
                spend.
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>

    <!-- Recent Performance Table -->
    <div class="row q-mt-lg">
      <div class="col-12">
        <q-card class="no-shadow border-light">
          <q-card-section class="q-px-lg">
            <div class="text-h6 text-weight-bold q-mb-md">Top Performing Products</div>
            <q-markup-table flat class="no-border">
              <thead>
                <tr>
                  <th class="text-left">Product / Service</th>
                  <th class="text-left">Category</th>
                  <th class="text-right">Revenue</th>
                  <th class="text-right">Growth</th>
                  <th class="text-center">Status</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="product in products" :key="product.id">
                  <td class="text-left text-weight-bold">{{ product.name }}</td>
                  <td class="text-left text-grey-7">{{ product.category }}</td>
                  <td class="text-right">{{ product.revenue }}</td>
                  <td class="text-right text-green-7">{{ product.growth }}</td>
                  <td class="text-center">
                    <q-badge color="green-1" text-color="green-9" label="Active" />
                  </td>
                </tr>
              </tbody>
            </q-markup-table>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script setup>
const kpis = [
  { label: 'Total Revenue', value: '$128,400', trend: '12%', trendUp: true },
  { label: 'Avg Deal Size', value: '$4,200', trend: '5%', trendUp: true },
  { label: 'Conversion Rate', value: '3.2%', trend: '0.4%', trendUp: false },
  { label: 'Churn Rate', value: '1.8%', trend: 'Stable', trendUp: true },
]

const chartData = [
  { label: 'Jan', height: 40, amount: '$40k' },
  { label: 'Feb', height: 35, amount: '$35k' },
  { label: 'Mar', height: 60, amount: '$60k' },
  { label: 'Apr', height: 75, amount: '$75k' },
  { label: 'May', height: 50, amount: '$50k' },
  { label: 'Jun', height: 85, amount: '$85k' },
  { label: 'Jul', height: 65, amount: '$65k' },
  { label: 'Aug', height: 90, amount: '$90k' },
  { label: 'Sep', height: 55, amount: '$55k' },
  { label: 'Oct', height: 70, amount: '$70k' },
  { label: 'Nov', height: 95, amount: '$95k' },
  { label: 'Dec', height: 80, amount: '$80k' },
]

const sources = [
  { name: 'Direct Sales', value: 45, color: 'primary' },
  { name: 'Social Media (LinkedIn)', value: 30, color: 'secondary' },
  { name: 'Email Marketing', value: 15, color: 'accent' },
  { name: 'Referrals', value: 10, color: 'purple' },
]

const products = [
  { id: 1, name: 'Enterprise License', category: 'Software', revenue: '$85,000', growth: '+24%' },
  { id: 2, name: 'Consulting Package A', category: 'Services', revenue: '$32,000', growth: '+15%' },
  { id: 3, name: 'Maintenance Basic', category: 'Support', revenue: '$11,400', growth: '+5%' },
]
</script>

<style lang="scss" scoped>
.border-light {
  border: 1px solid #edf2f7;
}
.transition-height {
  transition: height 0.5s ease-in-out;
}
.transition-height:hover {
  opacity: 1 !important;
  background-color: $secondary !important;
}
</style>
