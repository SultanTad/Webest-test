<script setup lang="ts">
import { onMounted, ref, computed } from 'vue'
import type { Ref } from 'vue'

import PaginationComponent from '@/components/PaginationComponent.vue'
import SearchComponent from '@/components/SearchComponent.vue'

interface Tender {
  id: number
  title: string
  description: string
}

const allTenders: Ref<Tender[]> = ref([])
const page: Ref<number> = ref(1)
const allPages: Ref<number> = ref(0)
const search: Ref<string> = ref('')

const getTenders = async () => {
  let res = await fetch(`https://api.test-webest.ru/list/?page=${page.value}`)
  let data = await res.json()
  allPages.value = data.page_count
  allTenders.value = data.data
}

const searchTender = computed(() => {
  return allTenders.value.filter((tender) => {
    return tender.title.includes(search.value)
  })
})

const changePage = (pageNumber: number) => {
  page.value = pageNumber
  getTenders()
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

const nextPage = () => {
  page.value++
  getTenders()
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

onMounted(async () => {
  await getTenders()
})
</script>
<template>
  <main class="main">
    <section class="main-page">
      <div class="container">
        <div class="search-wrapper">
          <SearchComponent v-model="search" />
        </div>
        <TransitionGroup name="list" tag="ul" class="tenders-list">
          <router-link
            class="tender-link"
            :to="{ name: 'tender', params: { id: tender.id } }"
            v-for="tender in searchTender"
            :key="tender.id"
          >
            <li class="tender-item">
              <h3 class="tender-item__title">{{ tender.title }}</h3>
              <p class="tender-item__text">{{ tender.description }}</p>
            </li>
          </router-link>
        </TransitionGroup>
        <PaginationComponent
          :allPages="allPages"
          @change="changePage($event)"
          :page="page"
          @next="nextPage"
        />
      </div>
    </section>
  </main>
</template>

<style lang="scss">
.search-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}

.tenders-list {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px;
  margin: 50px 0;
}

.tender-item {
  color: rgb(40, 40, 44);
  background-color: rgb(243, 243, 246);
  padding: 10px;
  border-radius: 10px;
  height: 210px;
  &__title {
    font-family: 'BaronNeue';
    font-size: 16px;
    line-height: 110%;
    margin-bottom: 16px;
  }

  &__text {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 5;
    -webkit-box-orient: vertical;
    font-family: 'Mulish';
    font-size: 16px;
    line-height: 20px;
  }
}

.tender-link {
  width: 240px;
}

.list-enter-active,
.list-leave-active {
  transition: all 1s ease;
  transition-duration: 1s;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transition-duration: 1s;
  transform: translateX(30px);
}
</style>
