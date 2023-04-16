<script setup>
import { Menu, MenuButton, MenuItems } from '@headlessui/vue'
import CheckIcon from './icons/CheckIcon.vue';
import ChevronIcon from './icons/ChevronIcon.vue'
</script>
<template>
  <Menu as="div" class="relative inline-block text-left">
    <div>
      <MenuButton class="inline-flex w-full justify-center text-sm">
        Guest
        <ChevronIcon class="h-5 w-5" aria-hidden="true" />
      </MenuButton>
    </div>

    <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100" leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
      <MenuItems class="absolute right-0 z-10 sm:w-80 origin-top-right rounded-md bg-[#F6EFE6] shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
        <div class="py-1">
          <div :class="['bg-[#F6EFE6] hover:bg-[#FFF8EF] px-4 py-3 flex gap-3 cursor-pointer']">
            <div class="w-6 h-6 flex align-middle">
            </div>
            <div>
              <p class="text-md font-semibold">Guest</p>
              <p class="text-xs">Default access level, can leave tributes, share media and stories</p>
            </div>
          </div>
          <div :class="['bg-[#FFF8EF] hover:bg-[#FFF8EF] px-4 py-3 flex gap-3 cursor-pointer']">
            <div class="w-6 h-6 flex align-middle">
              <CheckIcon></CheckIcon>
            </div>
            <div>
              <p class="text-md font-semibold">Administrator</p>
              <p class="text-xs">Can control all aspects of the website, including moderating content posted by others and changing website settings</p>
            </div>
          </div>
        </div>
      </MenuItems>
    </transition>
  </Menu>
</template>
