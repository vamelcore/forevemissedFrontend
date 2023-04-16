<script setup>
import DropdownBlock from './DropdownBlock.vue';
import MinusIcon from './icons/MinusIcon.vue'
</script>

<template>
  <div class="flex justify-between items-center ml-6 my-1.5 gap-2">
    <div class="flex justify-between items-center h-12 bg-[#F6EFE6] rounded-md grow px-3">
      <div>
        <p class="text-base">John Wick</p>
        <p class="text-xs">example@email.com</p>
      </div>
      <div>
        <DropdownBlock></DropdownBlock>
      </div>
    </div>
    <button class="text-[#D1CAC1] hover:text-red-500">
      <MinusIcon></MinusIcon>
    </button>
  </div>
</template>