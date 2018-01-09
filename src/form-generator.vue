<template>
    <div>
        <div v-for="section in schema">
            <v-card>
                <v-card-title>{{section.label}}</v-card-title>
                <div v-for="field in section.fields">
                    <v-form-generator-field :field="field" :value="model[field.model]"/>
                </div>
            </v-card>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'v-form-generator',
    props: {
      'model': Object,
      'schema': Object
    },
    components: {
      'v-form-generator-field': require('./form-field.vue').default
    },
    data () {
      return {}
    },
    created: function () {
      // On load
    },
    methods: {
      onBlur: function () {
        console.info('blur')
      },
      onChange: function () {
        console.info('change')
      },
      onFocus: function () {
        console.info('focus')
      },
      onInput: function () {
        console.info('input')
      },
    }
  }
</script>
