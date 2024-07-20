<script setup lang="ts">
interface Props {
  page?: number
  allPages?: number
}

const props = defineProps<Props>()
</script>

<template>
  <div class="pagination">
    <button
      v-for="pageNumber in 10"
      :key="pageNumber"
      @click="$emit('change', pageNumber)"
      class="pagination__btn"
    >
      {{ pageNumber }}
    </button>
    <div>...</div>
    <button @click="$emit('change', props.allPages)" class="pagination__all-btns">
      {{ props.allPages }}
    </button>
    <img
      class="pagination__arrow"
      @click="$emit('next', props.page)"
      src="../assets/images/icons.png"
      alt=""
    />
  </div>
</template>

<style lang="scss">
.pagination {
  margin-bottom: 50px;
  display: flex;
  align-items: center;
  gap: 0 10px;

  & button {
    padding: 11px 17px;
    border-radius: 12px;
    border: none;
    cursor: pointer;
    font-family: 'Mulish';
    font-size: 18px;
    font-weight: 400;
    line-height: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  & button:active {
    background-color: #000;
    color: #fff;
  }

  & button,
  & img {
    width: 44px;
    height: 44px;
    border: 1px solid #e8e8e8;
    border-radius: 12px;
    cursor: pointer;
  }
}
</style>
