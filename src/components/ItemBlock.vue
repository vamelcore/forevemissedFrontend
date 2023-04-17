<script>
import DropdownBlock from './DropdownBlock.vue'
import MinusIcon from './icons/MinusIcon.vue'

export default {
  components: {
    DropdownBlock,
    MinusIcon
  },
  props: {
    recipient: {
      required: true,
      type: Object,
    },
    roles: {
      required: true,
      type: Object,
    },
    index: {
      required: true
    }
  },
  emits: [
    'remove', 
    'roleChange'
  ],
  methods: {
    roleChangeMethod(newRole) {
      this.$emit('roleChange', this.index, newRole);
    }
  }
}
</script>

<template>
  <div class="flex justify-between items-center ml-6 my-1.5 gap-2">
    <div class="flex justify-between items-center h-12 bg-[#F6EFE6] rounded-md grow px-3">
      <div>
        <p class="text-base">{{ recipient.name ?? recipient.email }}</p>
        <p class="text-xs" v-if="recipient.name">{{ recipient.email }}</p>
      </div>
      <div>
        <DropdownBlock v-if="roles.length"
          :recipient="recipient" 
          :roles="roles" 
          @roleChange="roleChangeMethod"
        ></DropdownBlock>
      </div>
    </div>
    <button class="text-[#D1CAC1] hover:text-red-500">
      <MinusIcon @click="$emit('remove')"></MinusIcon>
    </button>
  </div>
</template>