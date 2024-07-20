<script setup lang="ts">
import { useRoute } from 'vue-router'
import { onMounted, ref } from 'vue'
import type { Ref } from 'vue'

interface CurrentTender {
  id?: number
  title?: string
  description?: string
}

const currentTender: Ref<CurrentTender | null> = ref(null)
const route = useRoute()

const getCurrentTender = async () => {
  let res = await fetch(`https://api.test-webest.ru/element/?id=${route.params.id}`)
  currentTender.value = await res.json()
}

onMounted(async () => {
  await getCurrentTender()
})
</script>
<template>
  <Transition name="fade">
    <section v-if="currentTender" class="tender-page">
      <div class="container">
        <router-link to="/">
          <button class="btn-home">Home</button>
        </router-link>
        <h2 class="tender-page__title">{{ currentTender.title }}</h2>
        <p class="tender-page__text">{{ currentTender.description }}</p>
      </div>
    </section>
  </Transition>
</template>

<style lang="scss">
.tender-page {
  margin-top: 50px;
  &__title {
    font-family: 'BaronNeue';
    font-size: 30px;
    line-height: 110%;
    margin-bottom: 16px;
  }

  &__text {
    font-family: 'Mulish';
    font-size: 16px;
    line-height: 20px;
  }
}

.btn-home {
  margin-bottom: 20px;
  background-color: rgb(54, 106, 220);
  color: #fff;
  border: none;
  padding: 12px;
  border-radius: 10px;
  cursor: pointer;
  font-size: 16px;
  font-family: 'Mulish';
  line-height: 110%;
}

.btn-home:hover {
  background-color: rgb(38, 86, 192);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
