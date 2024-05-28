<!-- eslint-disable vue/no-mutating-props -->
<template>
  <woot-modal :show.sync="show" :on-close="onCancel" modal-type="right-aligned">
    <div class="h-auto overflow-auto flex flex-col">
      <woot-modal-header
        :header-title="`${$t('EDIT_CONTACT.TITLE')} - ${
          obfuscatePhoneNumber(contact.name) || contact.email
        }`"
        :header-content="$t('EDIT_CONTACT.DESC')"
      />
      <contact-form
        :contact="contact"
        :in-progress="uiFlags.isUpdating"
        :on-submit="onSubmit"
        @success="onSuccess"
        @cancel="onCancel"
      />
    </div>
  </woot-modal>
</template>

<script>
import { mapGetters } from 'vuex';
import ContactForm from './ContactForm.vue';

export default {
  components: {
    ContactForm,
  },
  props: {
    show: {
      type: Boolean,
      default: false,
    },
    contact: {
      type: Object,
      default: () => ({}),
    },
  },

  computed: {
    ...mapGetters({
      uiFlags: 'contacts/getUIFlags',
    }),
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
    onCancel() {
      this.$emit('cancel');
    },
    onSuccess() {
      this.$emit('cancel');
    },
    async onSubmit(contactItem) {
      await this.$store.dispatch('contacts/update', contactItem);
      await this.$store.dispatch(
        'contacts/fetchContactableInbox',
        this.contact.id
      );
    },
  },
};
</script>
