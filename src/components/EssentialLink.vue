<template>
  <q-item clickable v-bind="linkProps">
    <q-item-section v-if="props.icon" avatar>
      <q-icon :name="props.icon" />
    </q-item-section>

    <q-item-section>
      <q-item-label>{{ props.title }}</q-item-label>
      <q-item-label caption>{{ props.caption }}</q-item-label>
    </q-item-section>
  </q-item>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  title: {
    type: String,
    required: true,
  },

  caption: {
    type: String,
    default: '',
  },

  link: {
    type: String,
    default: '#',
  },

  icon: {
    type: String,
    default: '',
  },
})

const isExternal = computed(() => {
  return props.link.startsWith('http')
})

const linkProps = computed(() => {
  if (isExternal.value) {
    return {
      tag: 'a',
      target: '_blank',
      href: props.link,
    }
  }
  return {
    to: props.link,
  }
})
</script>
