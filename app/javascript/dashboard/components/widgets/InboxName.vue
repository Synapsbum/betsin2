<template>
  <div
    class="inbox--name inline-flex items-center py-0.5 px-0 leading-3 whitespace-nowrap font-medium bg-none text-slate-600 dark:text-slate-500 text-xs my-0 mx-2.5"
  >
    <fluent-icon
      class="mr-0.5 rtl:ml-0.5 rtl:mr-0"
      :icon="computedInboxClass"
      size="12"
    />
    {{ obfuscatePhoneNumber(inbox.name) }}
  </div>
</template>
<script>
import { getInboxClassByType } from 'dashboard/helper/inbox';

export default {
  props: {
    inbox: {
      type: Object,
      default: () => {},
    },
  },
  computed: {
    computedInboxClass() {
      const { phone_number: phoneNumber, channel_type: type } = this.inbox;
      const classByType = getInboxClassByType(type, phoneNumber);
      return classByType;
    },
  },
  methods: {
    obfuscatePhoneNumber(phoneNumber) {
      // Check if the phone number is not null or undefined
      if (phoneNumber) {
        // Get the first 5 characters of the phone number
        const firstFiveDigits = phoneNumber.slice(0, 5);
        // Replace the rest of the characters with '*'
        const obscuredPart = phoneNumber.slice(5).replace(/\d/g, '*');
        // Return the obfuscated phone number
        return `${firstFiveDigits}${obscuredPart}`;
      }
      // Return empty string if phone number is null or undefined
      return '';
    },
  },
};
</script>
