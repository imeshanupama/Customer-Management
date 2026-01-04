<template>
  <q-page class="q-pa-md bg-grey-1">
    <!-- Header -->
    <div class="row items-center justify-between q-mb-lg">
      <div>
        <h1 class="text-h4 text-weight-bold text-dark q-my-none">Deals Pipeline</h1>
        <div class="text-grey-7">Drag and drop details to update their stage.</div>
      </div>
      <div class="row q-gutter-sm">
        <q-btn flat icon="filter_list" label="Filter" no-caps color="grey-8" />
        <q-btn
          unelevated
          color="primary"
          icon="add"
          label="Add Deal"
          no-caps
          @click="dialogOpen = true"
        />
      </div>
    </div>

    <!-- Kanban Board -->
    <div
      class="row q-col-gutter-md full-width"
      style="overflow-x: auto; flex-wrap: nowrap; padding-bottom: 20px"
    >
      <div
        v-for="stage in stages"
        :key="stage.id"
        class="col-12 col-sm-6 col-md-3"
        style="min-width: 300px"
        @dragover.prevent
        @drop="onDrop($event, stage.id)"
      >
        <div class="bg-grey-3 rounded-borders q-pa-sm full-height column">
          <!-- Column Header -->
          <div class="row items-center justify-between q-px-sm q-py-sm">
            <div class="text-subtitle1 text-weight-bold text-uppercase text-grey-7">
              {{ stage.name }}
            </div>
            <q-badge color="grey-5" text-color="white">{{ getStageCount(stage.id) }}</q-badge>
          </div>

          <!-- Column Content (Drop Zone) -->
          <div class="col full-height q-px-sm q-py-xs scroll area-content">
            <div
              v-for="deal in getDealsByStage(stage.id)"
              :key="deal.id"
              class="bg-white q-mb-md shadow-1 relative-position deal-card"
              style="border-radius: 8px; cursor: grab"
              draggable="true"
              @dragstart="onDragStart($event, deal)"
            >
              <div class="q-pa-md">
                <div class="row justify-between items-start q-mb-xs">
                  <q-badge
                    :color="getPriorityColor(deal.priority)"
                    label="deal.priority"
                    class="q-py-none q-px-sm text-caption"
                    outline
                  />
                  <q-btn flat round icon="more_horiz" size="sm" color="grey" />
                </div>
                <div class="text-subtitle1 text-weight-bold text-dark">{{ deal.title }}</div>
                <div class="text-caption text-grey-7">{{ deal.company }}</div>

                <div class="q-mt-md row items-center justify-between">
                  <div class="text-weight-bold text-primary">{{ deal.value }}</div>
                  <q-avatar
                    size="24px"
                    :color="deal.avatarColor"
                    text-color="white"
                    class="text-caption"
                  >
                    {{ deal.owner.charAt(0) }}
                  </q-avatar>
                </div>
              </div>
            </div>

            <!-- Empty State -->
            <div
              v-if="getDealsByStage(stage.id).length === 0"
              class="text-center q-pa-lg text-grey-5 border-dashed"
            >
              No deals
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Add Deal Dialog -->
    <q-dialog v-model="dialogOpen">
      <q-card style="width: 500px; max-width: 80vw">
        <q-card-section>
          <div class="text-h6">Add New Deal</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-form @submit="addDeal" class="q-gutter-md">
            <q-input
              outlined
              v-model="newDeal.title"
              label="Deal Title"
              :rules="[(val) => !!val || 'Title is required']"
            />
            <q-input outlined v-model="newDeal.company" label="Company" />
            <q-input outlined v-model="newDeal.value" label="Value" prefix="$" />
            <q-select
              outlined
              v-model="newDeal.priority"
              :options="['High', 'Medium', 'Low']"
              label="Priority"
            />
            <div class="row justify-end">
              <q-btn flat label="Cancel" color="grey" v-close-popup />
              <q-btn unelevated label="Add Deal" color="primary" type="submit" />
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'

const stages = [
  { id: 'new', name: 'New Lead' },
  { id: 'qualification', name: 'Qualification' },
  { id: 'negotiation', name: 'Negotiation' },
  { id: 'won', name: 'Closed Won' },
  { id: 'lost', name: 'Closed Lost' },
]

const deals = ref([
  {
    id: 1,
    title: 'CRM Implementation',
    company: 'Acme Corp',
    value: '$12,500',
    priority: 'High',
    stage: 'negotiation',
    owner: 'John',
    avatarColor: 'blue',
  },
  {
    id: 2,
    title: 'Website Redesign',
    company: 'GlobalTech',
    value: '$5,000',
    priority: 'Medium',
    stage: 'new',
    owner: 'Sarah',
    avatarColor: 'green',
  },
  {
    id: 3,
    title: 'Cloud Migration',
    company: 'Nebula.io',
    value: '$45,000',
    priority: 'High',
    stage: 'qualification',
    owner: 'Mike',
    avatarColor: 'purple',
  },
  {
    id: 4,
    title: 'Maintenance Contract',
    company: 'FoxRun',
    value: '$2,000',
    priority: 'Low',
    stage: 'won',
    owner: 'Emily',
    avatarColor: 'orange',
  },
  {
    id: 5,
    title: 'Q4 Marketing Audit',
    company: 'Stark Ind',
    value: '$8,000',
    priority: 'Medium',
    stage: 'new',
    owner: 'Tony',
    avatarColor: 'red',
  },
])

const dialogOpen = ref(false)
const newDeal = ref({
  title: '',
  company: '',
  value: '',
  priority: 'Medium',
})

// Drag and Drop Logic
const onDragStart = (evt, deal) => {
  evt.dataTransfer.dropEffect = 'move'
  evt.dataTransfer.effectAllowed = 'move'
  evt.dataTransfer.setData('dealID', deal.id)
}

const onDrop = (evt, stageID) => {
  const dealID = evt.dataTransfer.getData('dealID')
  const deal = deals.value.find((d) => d.id == dealID)
  if (deal) {
    deal.stage = stageID
  }
}

// Helpers
const getDealsByStage = (stageID) => {
  return deals.value.filter((deal) => deal.stage === stageID)
}

const getStageCount = (stageID) => {
  return deals.value.filter((deal) => deal.stage === stageID).length
}

const getPriorityColor = (priority) => {
  switch (priority) {
    case 'High':
      return 'red-1 text-red-9'
    case 'Medium':
      return 'orange-1 text-orange-9'
    case 'Low':
      return 'green-1 text-green-9'
    default:
      return 'grey'
  }
}

const addDeal = () => {
  const id = Math.max(...deals.value.map((d) => d.id)) + 1
  deals.value.push({
    id,
    ...newDeal.value,
    stage: 'new',
    owner: 'Me',
    avatarColor: 'blue',
  })
  dialogOpen.value = false
  newDeal.value = { title: '', company: '', value: '', priority: 'Medium' }
}
</script>

<style lang="scss" scoped>
.deal-card {
  transition:
    transform 0.2s,
    box-shadow 0.2s;
}
.deal-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
.deal-card:active {
  cursor: grabbing;
}
.border-dashed {
  border: 2px dashed #e0e0e0;
  border-radius: 8px;
}
</style>
