<template>
  <q-page class="q-pa-md bg-grey-1">
    <!-- Page Header -->
    <div class="row items-center justify-between q-mb-lg">
      <div>
        <h1 class="text-h4 text-weight-bold text-dark q-my-none">Customers</h1>
        <div class="text-grey-7">Manage your relationships and contacts.</div>
      </div>
      <q-btn
        unelevated
        color="primary"
        icon="add"
        label="Add Customer"
        no-caps
        @click="openAddCustomerDialog"
      />
    </div>

    <!-- Customer Table Card -->
    <q-card class="no-shadow border-light">
      <q-card-section>
        <div class="row q-col-gutter-md justify-between items-center">
          <div class="col-12 col-sm-4">
            <q-input
              v-model="search"
              dense
              outlined
              placeholder="Search customers..."
              class="bg-white"
            >
              <template v-slot:prepend>
                <q-icon name="search" />
              </template>
            </q-input>
          </div>
          <div class="col-12 col-sm-auto row q-gutter-sm">
            <q-btn outline icon="filter_list" label="Filter" no-caps color="grey-8" />
            <q-btn outline icon="file_download" label="Export" no-caps color="grey-8" />
          </div>
        </div>
      </q-card-section>

      <q-table
        :rows="customers"
        :columns="columns"
        row-key="id"
        :filter="search"
        flat
        class="customer-table"
        :pagination="pagination"
      >
        <!-- Body Slots for Custom Styling -->
        <template v-slot:body-cell-name="props">
          <q-td :props="props">
            <div class="row items-center">
              <q-avatar
                size="32px"
                :color="props.row.avatarColor"
                text-color="white"
                class="q-mr-sm"
              >
                {{ props.row.name.charAt(0) }}
              </q-avatar>
              <div>
                <div
                  class="text-weight-bold cursor-pointer text-primary"
                  @click="router.push(`/customers/${props.row.id}`)"
                >
                  {{ props.row.name }}
                </div>
                <div class="text-caption text-grey">{{ props.row.email }}</div>
              </div>
            </div>
          </q-td>
        </template>

        <template v-slot:body-cell-status="props">
          <q-td :props="props">
            <q-chip
              size="sm"
              :color="getStatusColor(props.row.status).bg"
              :text-color="getStatusColor(props.row.status).text"
              class="text-weight-bold"
            >
              {{ props.row.status }}
            </q-chip>
          </q-td>
        </template>

        <template v-slot:body-cell-actions="props">
          <q-td :props="props">
            <q-btn
              flat
              round
              color="grey-7"
              icon="edit"
              size="sm"
              @click="editCustomer(props.row)"
            />
            <q-btn
              flat
              round
              color="negative"
              icon="delete"
              size="sm"
              @click="deleteCustomer(props.row)"
            />
          </q-td>
        </template>
      </q-table>
    </q-card>

    <!-- Add/Edit Customer Dialog -->
    <q-dialog v-model="dialogOpen">
      <q-card style="width: 500px; max-width: 80vw">
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6 text-weight-bold">
            {{ isEditing ? 'Edit Customer' : 'Add New Customer' }}
          </div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-card-section class="q-pt-md">
          <q-form @submit="saveCustomer" class="q-gutter-md">
            <q-input
              outlined
              v-model="form.name"
              label="Full Name"
              :rules="[(val) => !!val || 'Name is required']"
            />
            <q-input
              outlined
              v-model="form.email"
              label="Email Address"
              type="email"
              :rules="[(val) => !!val || 'Email is required']"
            />
            <q-input outlined v-model="form.phone" label="Phone Number" mask="(###) ### - ####" />
            <q-input outlined v-model="form.company" label="Company" />

            <q-select
              outlined
              v-model="form.status"
              :options="['Active', 'Inactive', 'Lead', 'Churned']"
              label="Status"
            />

            <div class="row justify-end q-mt-lg">
              <q-btn label="Cancel" color="grey-7" flat v-close-popup class="q-mr-sm" />
              <q-btn
                :label="isEditing ? 'Update Customer' : 'Save Customer'"
                type="submit"
                color="primary"
                unelevated
              />
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useQuasar } from 'quasar'
import { useRouter } from 'vue-router'

const $q = useQuasar()
const router = useRouter()

// State
const search = ref('')
const dialogOpen = ref(false)
const isEditing = ref(false)
const pagination = ref({ rowsPerPage: 10 })

// Form State
const form = reactive({
  id: null,
  name: '',
  email: '',
  phone: '',
  company: '',
  status: 'Active',
  avatarColor: 'blue',
})

// Data
const columns = [
  { name: 'name', label: 'Name / Email', align: 'left', field: (row) => row.name, sortable: true },
  { name: 'company', label: 'Company', align: 'left', field: 'company', sortable: true },
  { name: 'phone', label: 'Phone', align: 'left', field: 'phone' },
  { name: 'status', label: 'Status', align: 'left', field: 'status', sortable: true },
  { name: 'joined', label: 'Date Joined', align: 'left', field: 'joined', sortable: true },
  { name: 'actions', label: '', align: 'right' },
]

const customers = ref([
  {
    id: 1,
    name: 'John Doe',
    email: 'john@acme.com',
    company: 'Acme Corp',
    phone: '(555) 123-4567',
    status: 'Active',
    joined: '2023-08-15',
    avatarColor: 'blue-8',
  },
  {
    id: 2,
    name: 'Sarah Smith',
    email: 'sarah@global.tech',
    company: 'GlobalTech',
    phone: '(555) 987-6543',
    status: 'Lead',
    joined: '2023-09-02',
    avatarColor: 'green-8',
  },
  {
    id: 3,
    name: 'Michael Brown',
    email: 'm.brown@nebula.io',
    company: 'Nebula',
    phone: '(555) 456-7890',
    status: 'Inactive',
    joined: '2023-01-20',
    avatarColor: 'purple-8',
  },
  {
    id: 4,
    name: 'Emily Davis',
    email: 'emily@foxrun.co',
    company: 'FoxRun',
    phone: '(555) 222-3333',
    status: 'Active',
    joined: '2023-11-05',
    avatarColor: 'orange-8',
  },
  {
    id: 5,
    name: 'David Wilson',
    email: 'david@circle.io',
    company: 'Circle.io',
    phone: '(555) 777-8888',
    status: 'Churned',
    joined: '2022-12-10',
    avatarColor: 'red-8',
  },
])

// Methods
const getStatusColor = (status) => {
  switch (status) {
    case 'Active':
      return { bg: 'green-1', text: 'green-9' }
    case 'Lead':
      return { bg: 'blue-1', text: 'blue-9' }
    case 'Inactive':
      return { bg: 'grey-2', text: 'grey-8' }
    case 'Churned':
      return { bg: 'red-1', text: 'red-9' }
    default:
      return { bg: 'grey-1', text: 'dark' }
  }
}

const openAddCustomerDialog = () => {
  isEditing.value = false
  resetForm()
  dialogOpen.value = true
}

const editCustomer = (row) => {
  isEditing.value = true
  Object.assign(form, row)
  dialogOpen.value = true
}

const resetForm = () => {
  form.id = null
  form.name = ''
  form.email = ''
  form.phone = ''
  form.company = ''
  form.status = 'Active'
  form.avatarColor = 'blue'
}

const saveCustomer = () => {
  if (isEditing.value) {
    // Update existing
    const index = customers.value.findIndex((c) => c.id === form.id)
    if (index !== -1) {
      customers.value[index] = { ...form }
      $q.notify({ type: 'positive', message: 'Customer updated successfully' })
    }
  } else {
    // Add new
    const newId = Math.max(...customers.value.map((c) => c.id)) + 1
    const today = new Date().toISOString().split('T')[0]
    customers.value.unshift({ ...form, id: newId, joined: today, avatarColor: 'teal' })
    $q.notify({ type: 'positive', message: 'Customer added successfully' })
  }
  dialogOpen.value = false
}

const deleteCustomer = (row) => {
  $q.dialog({
    title: 'Confirm',
    message: `Are you sure you want to delete ${row.name}?`,
    cancel: true,
    persistent: true,
  }).onOk(() => {
    customers.value = customers.value.filter((c) => c.id !== row.id)
    $q.notify({ type: 'negative', message: 'Customer deleted' })
  })
}
</script>

<style lang="scss" scoped>
.border-light {
  border: 1px solid #edf2f7;
}
</style>
