<template>
  <div class="contact-fields">
    <h3 class="text-lg title">
      {{ $t('CONTACTS_PAGE.FIELDS') }}
    </h3>
    <attribute
      :label="$t('CONTACT_PANEL.EMAIL_ADDRESS')"
      icon="mail"
      emoji=""
      :value="contact.email"
      :show-edit="true"
      @update="onEmailUpdate"
    />
    <attribute
      :label="$t('CONTACT_PANEL.PHONE_NUMBER')"
      icon="call"
      emoji=""
      :value="obfuscatePhoneNumber(contact.phone_number)"
      :show-edit="true"
      @update="onPhoneUpdate"
    />
    <attribute
      v-if="additionalAttributes.location"
      :label="$t('CONTACT_PANEL.LOCATION')"
      icon="map"
      emoji="🌍"
      :value="additionalAttributes.location"
      :show-edit="true"
      @update="onLocationUpdate"
    />
    <div
      v-for="attribute in customAttributekeys"
      :key="attribute"
      class="custom-attribute--row"
    >
      <attribute
        :label="attribute"
        icon="chevron-right"
        :value="customAttributes[attribute]"
        :show-edit="true"
        @update="value => onCustomAttributeUpdate(attribute, value)"
      />
    </div>
    <woot-button
      size="small"
      variant="link"
      icon="add"
      @click="handleCustomCreate"
    >
      {{ $t('CUSTOM_ATTRIBUTES.ADD.TITLE') }}
    </woot-button>
  </div>
</template>
<script>
import Attribute from './ContactAttribute.vue';

export default {
  components: { Attribute },
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
    company() {
      const { company = {} } = this.contact;
      return company;
    },
    customAttributes() {
      const { custom_attributes: customAttributes = {} } = this.contact;
      return customAttributes;
    },
    customAttributekeys() {
      return Object.keys(this.customAttributes).filter(key => {
        const value = this.customAttributes[key];
        return value !== null && value !== undefined;
      });
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
    onEmailUpdate(value) {
      this.$emit('update', { email: value });
    },
    onPhoneUpdate(value) {
      this.$emit('update', { phone_number: value });
    },
    onLocationUpdate(value) {
      this.$emit('update', { location: value });
    },
    handleCustomCreate() {
      this.$emit('create-attribute');
    },
    onCustomAttributeUpdate(key, value) {
      this.$emit('update', { custom_attributes: { [key]: value } });
    },
  },
};
</script>

<style scoped lang="scss">
.contact-fields {
  margin-top: var(--space-medium);
}

.title {
  margin-bottom: var(--space-normal);
}
</style>
