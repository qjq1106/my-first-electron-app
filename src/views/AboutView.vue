<script setup lang="ts">
import { useCounterStore } from '@/stores/counter'

const counter = useCounterStore()
</script>

<template>
  <h1>关于</h1>
  <div class="card">
    <p>计数：{{ counter.count }}（双倍：{{ counter.doubleCount }}）</p>
    <button type="button" @click="counter.increment()">点击 +1</button>
  </div>
  <p class="nav-link">
    <router-link to="/">返回首页</router-link>
  </p>
</template>

<style scoped>
.card {
  padding: 1.5em;
}
.nav-link {
  margin-top: 1.5em;
}
</style>
