<script setup>
  import { ref } from 'vue'
  import Dashboard from './dashboard.vue'
  import Content from './content.vue'

  const pathname = window.location.pathname

  const newContentId = ref('')

  async function createNewContent() {
    newContentId.value = Agent.uuid()
    const name = prompt('Name your content:')
    const content = await Agent.state(newContentId.value)
    content.name = name
    content.reference = {
      dashboard: window.location.host + '/dashboard'
    }
  }
</script>

<template>
  <Suspense>
    <div>
      <div v-if="pathname === '/'">
        <button @click="createNewContent">Create New Content</button>
        <div v-if="newContentId">
          new content id:
          <a :href="`/${newContentId}`">{{ newContentId }}
          </a>
        </div>
      </div>
      <Dashboard v-else-if="pathname === '/dashboard'" />
      <Content
        v-else
        :id="pathname.slice(1)"
      />
    </div>
  </Suspense>
</template>

<style scoped>
</style>
