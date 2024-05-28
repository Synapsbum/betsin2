<template>
  <search-result-section
    :title="$t('SEARCH.SECTION.CONTACTS')"
    :empty="!contacts.length"
    :query="query"
    :show-title="showTitle"
    :is-fetching="isFetching"
  >
    <ul v-if="contacts.length" class="search-list">
      <li v-for="contact in contacts" :key="contact.id">
        <search-result-contact-item
          :id="contact.id"
          :name="obfuscatePhoneNumber(contact.name)"
          :email="contact.email"
          :phone="obfuscatePhoneNumber(contact.phone_number)"
          :account-id="accountId"
          :thumbnail="contact.thumbnail"
        />
      </li>
    </ul>
  </search-result-section>
</template>

<script>
import { mapGetters } from 'vuex';

import SearchResultSection from './SearchResultSection.vue';
import SearchResultContactItem from './SearchResultContactItem.vue';

export default {
  components: {
    SearchResultSection,
    SearchResultContactItem,
  },
  props: {
    contacts: {
      type: Array,
      default: () => [],
    },
    query: {
      type: String,
      default: '',
    },
    isFetching: {
      type: Boolean,
      default: false,
    },
    showTitle: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    ...mapGetters({
      accountId: 'getCurrentAccountId',
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
  },
};
</script>
