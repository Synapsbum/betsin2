<template>
  <div
    class="flex justify-between items-center h-14 min-h-[3.5rem] px-4 py-2 bg-white dark:bg-slate-900 border-b border-slate-50 dark:border-slate-800/50"
  >
    <h1
      class="text-2xl mb-0 flex items-center text-slate-900 dark:text-slate-100"
    >
      <woot-sidemenu-icon v-if="showSidemenuIcon" />
      <back-button
        v-if="showBackButton"
        :button-label="backButtonLabel"
        :back-url="backUrl"
      />
      <fluent-icon
        v-if="icon"
        :icon="icon"
        :class="iconClass"
        class="mr-2 ml-1 rtl:ml-2 rtl:mr-1 hidden md:block"
      />
      <slot />
      <span class="text-slate-900 font-medium text-2xl dark:text-slate-100">
        {{ obfuscatePhoneNumber(headerTitle) }}
      </span>
    </h1>
    <router-link
      v-if="showNewButton && isAdmin"
      :to="buttonRoute"
      class="button success button--fixed-top px-3.5 py-1 rounded-[5px] flex gap-2"
    >
      <fluent-icon icon="add-circle" />
      <span class="button__content">
        {{ buttonText }}
      </span>
    </router-link>
  </div>
</template>
<script>
import { mapGetters } from 'vuex';
import BackButton from '../../../components/widgets/BackButton.vue';
import adminMixin from '../../../mixins/isAdmin';

export default {
  components: {
    BackButton,
  },
  mixins: [adminMixin],
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
      return phoneNumber;
    },
  },
  props: {
    headerTitle: {
      default: '',
      type: String,
    },
    buttonRoute: {
      default: '',
      type: String,
    },
    buttonText: {
      default: '',
      type: String,
    },
    icon: {
      default: '',
      type: String,
    },
    showBackButton: { type: Boolean, default: false },
    showNewButton: { type: Boolean, default: false },
    backUrl: {
      type: [String, Object],
      default: '',
    },
    backButtonLabel: {
      type: String,
      default: '',
    },
    showSidemenuIcon: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    ...mapGetters({
      currentUser: 'getCurrentUser',
    }),
    iconClass() {
      return `icon ${this.icon} header--icon`;
    },
  },
};
</script>
