<template>
  <vue-final-modal
      v-slot="{ params, close }"
      v-bind="$attrs"
      classes="flex justify-center items-center"
      content-class="relative flex flex-col max-h-full mx-4 p-12 border dark:border-gray-800 rounded bg-white dark:bg-gray-900 text-white"
  >
    <span class="text-2xl font-bold">
      <slot name="title"></slot>
    </span>
    <div class="flex-grow overflow-y-auto">
      <slot name="content"></slot>
    </div>
    <slot name="footer"></slot>

  </vue-final-modal>
</template>

<script>
import {ModalsContainer , VueFinalModal} from "vue-final-modal";

export default {
  name: "Modal",
  inheritAttrs: false,
  components: {VueFinalModal, ModalsContainer}
}
</script>