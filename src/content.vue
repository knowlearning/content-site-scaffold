<template>
  <div>
    <button
      v-if="!embedded"
      @click="backToCreator()"
    >Back To Creator</button>
    <div
      class="grid"
      @click="handleClick"
      ref="gridEl"
    >
      <div
        class="dot"
        :style="{
          left: runstate.x + 'px',
          top: runstate.y + 'px',
          backgroundColor: `hsl(${runstate.hue}, 100%, 50%)`
        }"
      />
    </div>
  </div>
</template>

<script setup>
  import { ref, reactive } from 'vue'

  const embedded = Agent.embedded

  const props = defineProps({ id: String })

  const runstate = reactive(await Agent.state(`runstate-${props.id}`))

  const gridEl = ref(null)

  if (runstate.x === undefined) {
    runstate.x = 150
    runstate.y = 150
    runstate.hue = 0
    runstate.reference = {
      dashboard: window.location.host + '/dashboard'
    }
  }

  function handleClick(event) {
    const { left, top } = gridEl.value.getBoundingClientRect()

    runstate.x = event.clientX - left
    runstate.y = event.clientY - top

    runstate.hue = (runstate.hue + 30) % 360 // advance color each click
  }

  function backToCreator() {
    window.location = '/'
  }
</script>

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
