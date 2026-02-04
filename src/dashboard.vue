<script setup>
  import { reactive } from 'vue'

  const { variables: { users, assignment } } = await Agent.environment()
  const { content } = await Agent.state(assignment)

  const runstates = reactive({})

  users.forEach(async userId => {
    const { auth: { info: { name } } } = await Agent.environment(userId)
    runstates[userId] = {
      name
    }
    Agent
      .watch(
        `runstate-${content}`,
        ({ state }) => {
          runstates[userId].state = state
        },
        userId
      )

    console.log(runstates)
  })
</script>

<template>
  <div>
    <div
      class="grid"
      @click="handleClick"
      ref="gridEl"
    >
      <div
        v-for="{ state, name } in runstates"
        class="dot"
        :style="{
          left: state.x + 'px',
          top: state.y + 'px',
          backgroundColor: `hsl(${state.hue}, 100%, 50%)`
        }"
      />
    </div>
    <div v-for="{ name } in runstates"> {{ name }} </div>
  </div>
</template>

<style scoped>
.grid {
  width: 300px;
  height: 300px;
  border: 1px solid #999;
  position: relative;
}

.dot {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
}
</style>
