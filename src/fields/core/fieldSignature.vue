<template>
     <vueSignature ref="signature"/>
</template>
<script>
  import abstractField from "../abstractField"
  import VueSignature from 'vue-signature/src/lib/vue-signature'

  export default {
    mixins: [abstractField],
    fieldTypes: ['signature'],
    components: {
      'vueSignature': VueSignature
    },
    data () {
      return {
      }
    },
    methods: {
    }
  }
</script>
