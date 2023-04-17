<script>
import axios from 'axios'
import { Dialog as ModalDialog, DialogPanel, TransitionChild, TransitionRoot } from '@headlessui/vue'
import { useVuelidate } from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'
import CloseIcon from './icons/CloseIcon.vue'
import YahooIcon from './icons/YahooIcon.vue'
import GmailIcon from './icons/GmailIcon.vue'
import AolIcon from './icons/AolIcon.vue'
import OnedriveIcon from './icons/OnedriveIcon.vue'
import ItemBlock from './ItemBlock.vue'

export default {
  setup () {
    return { v$: useVuelidate() }
  },
  components: {
    ModalDialog, 
    DialogPanel, 
    TransitionChild, 
    TransitionRoot,
    CloseIcon,
    YahooIcon,
    GmailIcon,
    AolIcon,
    OnedriveIcon,
    ItemBlock
  },
  props: ['openWindow'],
  emits: ['update:openWindow'],
  computed: {
    open: {
      get() {
        return this.openWindow
      },
      set(value) {
        this.$emit('update:openWindow', value)
      }
    }
  },
  data: function(){
    return {
      recipientEmail: '',
      recipients: [],
      roles: [],
    }
  },
  methods: {
    getRoles() {
      axios
      .get(import.meta.env.VITE_API_URL+'/roles/list')
      .then((responce) => {
        if (responce?.data?.data ) {
          this.roles = responce.data.data
        }
      })
      .catch(error => {
        console.log(error);
      })
    },
    sendRecipients() {
      if (this.recipients.length) {
        axios
        .post(import.meta.env.VITE_API_URL+'/invitation/process', {data:this.recipients})
        .then((responce) => {
        if (responce?.data?.success ) {
          this.recipients = []
          this.open = false
        }
      })
      .catch(error => {
        console.log(error);
      })
      }
    },
    async addRecipient() {
      const defaultRole = this.roles.length ? this.roles[0].name : ''
      let recipient = {
        'email': this.recipientEmail,
        'name': 'Some Name',
        'role': defaultRole
      }

      const isValid = await this.v$.$validate()

      if (isValid) {
        this.recipients.push(recipient)
        this.recipientEmail = ''
        this.v$.$reset()
      }
    },
    roleChangeMethod(index, newRole) {
      this.recipients[index].role = newRole
    },
  },
  validations() {
    return {
      recipientEmail: { required, email }
    }
  },
  mounted() {
    this.getRoles()
  }
}
</script>

<template>
  <TransitionRoot as="template" :show="open">
    <ModalDialog as="div" class="relative z-10" @close="open = false">
      <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
      </TransitionChild>
      <div class="fixed inset-0 z-10 overflow-y-auto">
        <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
          <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95" enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200" leave-from="opacity-100 translate-y-0 sm:scale-100" leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
            <DialogPanel class="relative transform overflow-hidden rounded-[5px] bg-[#FFF8EF] text-left shadow-xl transition-all sm:my-8 sm:w-[600px] text-[#3C1F1D]">
              <div class="bg-[#FFF8EF]">
                <div class="flex justify-between items-center m-6">
                  <h3 class="text-2xl font-semibold">Invite others</h3>
                  <button type="button" class="inline-flex items-center justify-center text-[#3C1F1D] no-focus" @click="open = false">
                    <CloseIcon></CloseIcon>
                  </button>
                </div>
                <div class="flex justify-between items-center mx-6 gap-2">
                  <input :class="v$.recipientEmail.$error ? 'border-red-700 border-2':'border-[#333333]/[.16]'" class="h-12 bg-white px-4 py-3 border rounded-md grow text-base" v-model.trim="recipientEmail" placeholder="Enter people E-mails" />
                  <button class="h-12 border border-[#D1CAC1] rounded-md px-6 py-3 text-base" @click="addRecipient()">Add</button>
                </div>
                <div class="flex justify-start items-center gap-3 m-6 mt-4">
                  <span class="text-base">or add from</span>
                  <YahooIcon></YahooIcon>
                  <GmailIcon></GmailIcon>
                  <AolIcon></AolIcon>
                  <OnedriveIcon></OnedriveIcon>
                </div>
                <hr class="w-100 bg-[#DFD8CF]" />
                  <div class="flex flex-col h-96 overflow-y-scroll py-1.5">
                    <ItemBlock 
                      v-for="(recipient, index) in recipients" 
                      :key="index" 
                      :recipient="recipient"
                      :index="index"
                      @remove="recipients.splice(index, 1)"
                      :roles="roles"
                      @roleChange="roleChangeMethod"
                    ></ItemBlock>
                  </div>
                <hr class="w-100 bg-[#DFD8CF]" />
                <div class="flex items-center m-6">
                  <input class="h-12 bg-white px-4 py-3 border rounded-md border-[#333333]/[.16] grow text-base" placeholder="Personal message (optional)" />
                </div>
                <div class="flex justify-end items-center m-6 gap-6">
                  <span class="text-base text-[#876A68]">{{ recipients.length }} {{ recipients.length === 1 ? 'recipient' : 'recipients' }}</span>
                  <button class="h-12 w-[160px] bg-gradient rounded-md px-12 py-3 text-base text-white" @click="sendRecipients()">Send</button>
                </div>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </ModalDialog>
  </TransitionRoot>
</template>
