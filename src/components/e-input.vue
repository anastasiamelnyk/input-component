<template>
  <div
    ref="inputContainerRef"
    :class="[
      'e-input',
      `e-input--${theme}`,
      {
      'e-input--error': error,
      'e-input--label-inline': isLabelInline,
      'e-input--label-animated': isLabelAnimated,
      'e-input--non-empty': !isInputEmpty
      }
    ]"
  >
    <div class="e-input__wrapper">
      <component
        :is="isMultiline ? 'textarea' : 'input'"
        :value="value"
        :type="type"
        :id="name"
        :rows="isMultiline ? rows : none"
        :placeholder="isLabelAnimated ? label : placeholder"
        class="e-input__control"
        @input="$emit('input', $event.target.value)"
      />
      <label
        :for="name"
        class="e-input__label"
      >
        {{label}}
      </label>
    </div>
    <transition name="slide">
      <div
        v-if="error"
        class="e-input__error"
      >
        {{error}}
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'EInput',

  props: {
    value: {
      type: String,
      default: '',
    },

    type: {
      type: String,
      default: 'text',
    },

    name: {
      type: String,
      required: true,
    },

    placeholder: {
      type: String,
      default: '',
    },

    label: {
      type: String,
      required: true,
    },

    isMultiline: {
      type: Boolean,
      default: false,
    },

    rows: {
      type: Number,
      default: 2,
    },

    error: {
      type: String,
      default: '',
    },

    theme: {
      type: String,
      default: 'light',
    },

    isLabelAnimated: {
      type: Boolean,
      default: false, 
    },
  },

  data: () => ({
    isLabelInline: false,
  }),

  computed: {
    isInputEmpty() {
      return this.isLabelAnimated && !this.value.length;
    },
  },

  mounted() {
    if (!this.isLabelAnimated) {
      this.checkLabelsPosition();
      window.addEventListener('resize', this.checkLabelsPosition);
    }
  },

  beforeDestroy() {
    if (!this.isLabelAnimated) {
      window.removeEventListener('resize', this.checkLabelsPosition);
    }
  },

  methods: {
    checkLabelsPosition() {
      if (this.$refs.inputContainerRef.clientWidth > 400) {
        this.isLabelInline = true;
      } else {
        this.isLabelInline = false;
      }
    },
  },
};
</script>

<style lang="scss">
@import '../assets/scss/variables';

.e-input {
  $self: '.e-input';

  width: 100%;
  padding-bottom: 16px;
  position: relative;

  &__wrapper {
    display: flex;
    flex-direction: column-reverse;
  }

  &__label {
    margin-bottom: 4px;
    flex-shrink: 0;
  }

  &__control {
    border-radius: 5px;
    padding: 6px 12px;
    font: inherit;
    color: inherit;

    &:focus {
      outline: none;
    }
  }

  &__error {
    padding-top: 2px;
    font-size: 14px;
    line-height: 1.2;
    color: $color-accent-light;
  }

  &--light {
    #{$self}__control {
      border: 1px solid $gray-400;
      color: $gray-800;

      &:focus {
        box-shadow: 0 0 2px 0 $gray-500;
      }
    }
  }

  &--dark,
  &--gray {
    #{$self}__control {
      border: 1px solid $gray-600;
      color: $gray-200;

      &:focus {
        box-shadow: 0 0 2px 0 $gray-500;
      }
    }

    #{$self}__label {
      color: $gray-100;
    }
  }

  &--dark {
    background-color: $color-primary;

    #{$self}__control {
      background-color: $color-primary;
    }
  }

  &--gray {
    background-color: $gray-900;

    #{$self}__control {
      background-color: $gray-900;
    }
  }

  &--label-animated {
    &#{$self}--light {
      #{$self}__label,
      #{$self}__control::placeholder {
        color: $gray-600;
      }

      &#{$self}--non-empty #{$self}__label,
      &#{$self}--error #{$self}__label,
      #{$self}__control:focus ~ #{$self}__label {
        background-color: $white;
      }
    }

    &#{$self}--dark,
    &#{$self}--gray {
      #{$self}__label,
      #{$self}__control::placeholder {
        color: $gray-500;
      }
    }

    &#{$self}--dark {
      &#{$self}--non-empty #{$self}__label,
      &#{$self}--error #{$self}__label,
      #{$self}__control:focus ~ #{$self}__label {
        background-color: $color-primary;
      }
    }

    &#{$self}--gray {
      &#{$self}--non-empty #{$self}__label,
      &#{$self}--error #{$self}__label,
      #{$self}__control:focus ~ #{$self}__label {
        background-color: $gray-900;
      }
    }

    &#{$self}--error {
      #{$self}__label {
        top: -11px;
        opacity: 1;
        color: $color-accent-light;
      }
    }

    #{$self}__control:focus::placeholder {
      color: transparent;
      transition: color .3s;
    }

    #{$self}__label  {
      position: absolute;
      left: 9px;
      font-size: 14px;
      padding: 0 3px;
      opacity: 0;
      top: 7px;
      background-color: transparent;
      transition: top .4s, opacity .3s .1s, background-color .3s .1s;
    }

    &#{$self}--non-empty #{$self}__label,
    #{$self}__control:focus ~ #{$self}__label  {
      top: -11px;
      opacity: 1;
      transition: top .3s .1s, opacity .2s, background-color .2s;
    }
  }

  &--label-inline {
    #{$self}__wrapper {
      flex-direction: row-reverse;
      align-items: baseline;
    }

    #{$self}__label {
      width: 105px;
      margin-bottom: 0;
    }

    #{$self}__control {
      width: 100%;
    }

    #{$self}__error {
      padding-left: 105px;
    }
  }

  &--error {
    #{$self}__control {
      border-color: $color-accent-light;
      box-shadow: 0 0 2px 0 $color-accent-light;
    }
  }
}

.slide-enter-active,
.slide-leave-active {
  height: 18px;
  transition: height .2s;
}

.slide-leave-to,
.slide-enter {
  height: 0px;
}
</style>