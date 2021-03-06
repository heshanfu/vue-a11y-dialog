<template>
  <div :id="id" :class="classNames.base" ref="rootElement">
    <div
      tabIndex="-1"
      :class="classNames.overlay"
      @click="close" />
    <dialog :class="classNames.element" :aria-labelledby="titleId">
      <div role="document" :class="classNames.document">
        <button
          type="button"
          :aria-label="closeButtonLabel"
          @click="close"
          :class="classNames.closeButton">
          <slot name="closeButtonContent">
            {{ '\u00D7' }}
          </slot>
        </button>

        <h1 :id="fullTitleId" :class="classNames.title">
          <slot name="title" />
        </h1>

        <slot />
      </div>
    </dialog>
  </div>
</template>

<script>
  import A11yDialog from 'a11y-dialog'

  export default {
    name: 'VueA11yDialog',

    props: {
      id: { type: String, required: true },
      appRoot: { type: [String, Array], required: true },
      classNames: { type: Object, default: () => ({}) },
      titleId: { type: String },
      closeButtonLabel: { type: String, default: 'Close this dialog window' }
    },

    computed: {
      fullTitleId () {
        return this.titleId || this.id + '-title'
      }
    },

    data: () => ({
      dialog: null
    }),

    methods: {
      close () {
        this.dialog.hide()
      }
    },

    mounted () {
      this.dialog = new A11yDialog(this.$refs.rootElement, this.appRoot)
      this.$emit('dialog-ref', this.dialog)
    },

    destroyed () {
      if (this.dialog) {
        this.dialog.destroy()
      }

      this.$emit('dialog-ref')
    }
  }
</script>
