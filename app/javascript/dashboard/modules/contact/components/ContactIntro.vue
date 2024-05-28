<template>
  <div class="contact--intro">
    <thumbnail
      :src="contact.thumbnail"
      size="64px"
      :username="obfuscatePhoneNumber(contact.name)"
      :status="contact.availability_status"
    />

    <div class="contact--details">
      <h2 class="text-lg contact--name">
        {{ obfuscatePhoneNumber(contact.name) }}
      </h2>
      <h3 class="text-base contact--work">
        {{ contact.title }}
        <i v-if="company.name" class="icon ion-minus-round" />
        <span class="company-name">{{ company.name }}</span>
      </h3>
      <p v-if="additionalAttributes.description" class="contact--bio">
        {{ additionalAttributes.description }}
      </p>
      <social-icons :social-profiles="socialProfiles" />
    </div>
    <div class="contact-actions">
      <woot-button
        class="new-message"
        size="small expanded"
        icon="ion-paper-airplane"
        @click="onNewMessageClick"
      >
        {{ $t('CONTACT_PANEL.NEW_MESSAGE') }}
      </woot-button>
      <woot-button
        variant="hollow"
        size="small expanded"
        icon="edit"
        @click="onEditClick"
      >
        {{ $t('EDIT_CONTACT.BUTTON_LABEL') }}
      </woot-button>
    </div>
  </div>
</template>
<script>
import Thumbnail from 'dashboard/components/widgets/Thumbnail.vue';
import SocialIcons from 'dashboard/routes/dashboard/conversation/contact/SocialIcons.vue';

export default {
  components: {
    Thumbnail,
    SocialIcons,
  },
  props: {
    contact: {
      type: Object,
      default: () => ({}),
    },
  },

  computed: {
    additionalAttributes() {
      return this.contact.additional_attributes || {};
    },
    socialProfiles() {
      const {
        social_profiles: socialProfiles,
        screen_name: twitterScreenName,
      } = this.additionalAttributes;

      return { twitter: twitterScreenName, ...(socialProfiles || {}) };
    },
    company() {
      const { company = {} } = this.contact;
      return company;
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
      return phoneNumber;
    },
    onEditClick() {
      this.$emit('edit');
    },
    onNewMessageClick() {
      this.$emit('message');
    },
  },
};
</script>

<style scoped lang="scss">
.contact--details {
  margin-top: var(--space-small);
}

.contact--work {
  color: var(--color-body);

  .icon {
    font-size: var(--font-size-nano);
    vertical-align: middle;
  }
}

.contact--name {
  text-transform: capitalize;
  font-weight: var(--font-weight-bold);
}

.contact--bio {
  margin: var(--space-smaller) 0 0;
}

.button.new-message {
  margin-right: var(--space-small);
}

.contact-actions {
  display: flex;
  align-items: center;
  width: 100%;
  margin-top: var(--space-small);
}
</style>
