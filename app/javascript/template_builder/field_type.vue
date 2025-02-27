<template>
  <span
    class="dropdown"
    @mouseenter="renderDropdown = true"
    @touchstart="renderDropdown = true"
  >
    <slot>
      <label
        tabindex="0"
        :title="fieldNames[modelValue]"
        class="cursor-pointer"
      >
        <component
          :is="fieldIcons[modelValue]"
          :width="buttonWidth"
          :class="buttonClasses"
          :stroke-width="1.6"
        />
      </label>
    </slot>
    <ul
      v-if="editable && renderDropdown"
      tabindex="0"
      class="dropdown-content menu menu-xs p-2 shadow rounded-box w-52 z-10 mb-3"
      :class="menuClasses"
      @click="closeDropdown"
    >
      <template
        v-for="(icon, type) in fieldIcons"
        :key="type"
      >
        <li v-if="withPhone || withPayment || !['phone', 'payment'].includes(type)">
          <a
            href="#"
            class="text-sm py-1 px-2"
            :class="{ 'active': type === modelValue }"
            @click.prevent="$emit('update:model-value', type)"
          >
            <component
              :is="icon"
              :stroke-width="1.6"
              :width="20"
            />
            {{ fieldNames[type] }}
          </a>
        </li>
      </template>
    </ul>
  </span>
</template>

<script>
import { IconTextSize, IconWritingSign, IconCalendarEvent, IconPhoto, IconCheckbox, IconPaperclip, IconSelect, IconCircleDot, IconChecks, IconColumns3, IconPhoneCheck, IconLetterCaseUpper, IconCreditCard, IconRubberStamp } from '@tabler/icons-vue'

export default {
  name: 'FiledTypeDropdown',
  inject: ['withPhone', 'withPayment'],
  props: {
    modelValue: {
      type: String,
      required: true
    },
    menuClasses: {
      type: String,
      required: false,
      default: 'mt-1.5 bg-base-100'
    },
    buttonClasses: {
      type: String,
      required: false,
      default: ''
    },
    editable: {
      type: Boolean,
      required: false,
      default: true
    },
    buttonWidth: {
      type: Number,
      required: false,
      default: 18
    }
  },
  emits: ['update:model-value'],
  data () {
    return {
      renderDropdown: false
    }
  },
  computed: {
    fieldNames () {
      return {
        text: 'Text',
        signature: 'Signature',
        initials: 'Initials',
        date: 'Date',
        image: 'Image',
        file: 'File',
        select: 'Select',
        checkbox: 'Checkbox',
        multiple: 'Multiple',
        radio: 'Radio',
        cells: 'Cells',
        stamp: 'Stamp',
        payment: 'Payment',
        phone: 'Phone'
      }
    },
    fieldIcons () {
      return {
        text: IconTextSize,
        signature: IconWritingSign,
        initials: IconLetterCaseUpper,
        date: IconCalendarEvent,
        image: IconPhoto,
        file: IconPaperclip,
        checkbox: IconCheckbox,
        radio: IconCircleDot,
        select: IconSelect,
        multiple: IconChecks,
        cells: IconColumns3,
        stamp: IconRubberStamp,
        payment: IconCreditCard,
        phone: IconPhoneCheck
      }
    }
  },
  methods: {
    closeDropdown () {
      document.activeElement.blur()
    }
  }
}
</script>
