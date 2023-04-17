<script>
import { Menu as DropDownMenu, MenuButton, MenuItems } from '@headlessui/vue'
import CheckIcon from './icons/CheckIcon.vue'
import ChevronIcon from './icons/ChevronIcon.vue'

export default {
  components: {
    DropDownMenu,
    MenuButton,
    MenuItems,
    CheckIcon,
    ChevronIcon
  },
  props: {
    recipient: {
      required: true,
      type: Object,
    },
    roles: {
      required: true,
      type: Object,
    }
  },
  emits: ['roleChange'],
  data: function() {
    return {
      active: false
    }
  },
}
</script>
<template>
  <DropDownMenu as="div" class="relative inline-block text-left">
    <div>
      <MenuButton class="inline-flex w-full justify-center text-sm">
        {{ recipient.role }}
        <ChevronIcon class="h-5 w-5" aria-hidden="true" />
      </MenuButton>
    </div>

    <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100" leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
      <MenuItems class="absolute right-0 z-10 sm:w-80 origin-top-right rounded-md bg-[#F6EFE6] shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
        <div class="py-1">
          <template v-for="(role, index) in roles" :key="index">
            <div
              class="hover:bg-[#FFF8EF] px-4 py-3 flex gap-3 cursor-pointer"
              :class="[role.name == recipient.role ? 'bg-[#FFF8EF]' : 'bg-[#F6EFE6]']"
              @click="$emit('roleChange', role.name)" 
            >
              <div class="w-6 h-6 flex items-center">
                <CheckIcon :class="[role.name == recipient.role ? 'opacity-1' : 'opacity-0']"></CheckIcon>
              </div>
              <div>
                <p class="text-md font-semibold">{{ role.name }}</p>
                <p class="text-xs">{{ role.description }}</p>
              </div>
            </div>
          </template>
        </div>
      </MenuItems>
    </transition>
  </DropDownMenu>
</template>
