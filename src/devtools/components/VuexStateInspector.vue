<template>
  <div class="vuex-state-inspector">
    <data-field
      v-for="(key, value) of activeState"
      track-by="$index"
      :field="{ key: key, value: value }"
      :expanded="key === 'state'"
      :depth="0">
    </data-field>
  </div>
</template>

<script>
import CircularJSON from 'circular-json'
import DataField from './DataField.vue'

export default {
  components: {
    DataField
  },
  vuex: {
    state: {
      activeState ({ vuex: { base, history, activeIndex }}) {
        const entry = history[activeIndex]
        const res = {}
        if (entry) {
          res.type = entry.mutation.type
          res.payload = entry.mutation.payload
        }
        res.state = CircularJSON.parse(entry ? entry.state : base)
        return res
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
.vuex-state-inspector
  padding 15px 20px
</style>