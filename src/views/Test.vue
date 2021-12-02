<template>
  <div id="test-box">test page - {{ num }}</div>
</template>

<script>
import '@/styles/Test.less'

export default {
  name: 'TestPage',
  data() {
    return {
      num: 100,
    }
  },
}
</script>
