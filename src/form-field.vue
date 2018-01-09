<template>
    <div>
        <div v-if="field.field_id === 'unit_field' ||
					field.field_id === 'single_line' ||
					field.field_id === 'ssn' ||
					field.field_id === 'phone'
					">
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.editable"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            />
        </div>
        <div v-else-if="field.field_id === 'multi_line'
					">
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.editable"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    multi-line
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            />
        </div>
        <div v-else-if="field.field_id === 'email'">
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :rules="validationRules.email"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            />
        </div>
        <div v-else-if="field.field_id === 'date'">
            xxx
            <v-menu
                    lazy
                    :close-on-content-click="true"
                    v-model="menu"
                    transition="scale-transition"
                    offset-y
                    full-width
                    :nudge-right="40"
                    max-width="290px"
                    min-width="290px"
            >
                <v-text-field
                        v-model="dateFormatted"
                        :label="field.label"
                        slot="activator"
                        :required="field.required"
                        :readonly="field.readonly"
                        :disabled="field.disabled"
                        :placeholder="field.placeholder"
                        @blur="localValue = parseDate(dateFormatted)"
                        @change="onChange"
                        @focus="onFocus"
                        @input="onInput"
                />
                <v-date-picker v-model="localValue" @input="dateFormatted = formatDate($event)" no-title scrollable
                               actions>
                    <template slot-scope="{ save, cancel }">
                        <v-card-actions>
                            <v-spacer/>
                            <v-btn flat color="primary" @click="cancel">Cancel</v-btn>
                            <v-btn flat color="primary" @click="save">OK</v-btn>
                        </v-card-actions>
                    </template>
                </v-date-picker>
            </v-menu>
        </div>
        <div v-else-if="field.field_id === 'choice' || field.field_id === 'state'">
            <v-select
                    v-model="localValue"
                    :items="field.choices"
                    item-text="label"
                    item-value="value"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    single-line
                    bottom

            />
        </div>
        <div v-else-if="field.field_id === 'text_display'">
            <div v-html="field.label"></div>
        </div>
        <div v-else-if="field.field_id === 'file'">
            No File Field Right Now
        </div>
        <div v-else-if="field.field_id === 'signature'">
            No Signature Field Right Now
        </div>

        <div v-else-if="field.field_id === 'password'">
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :append-icon="field.passwordVisible ? 'visibility_off' : 'visibility'"
                    :append-icon-cb="appendPasswordIconCheckbox()"
                    :type="field.passwordVisible ? 'text' : 'password'"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            />
        </div>

        <div v-else-if="field.field_id == 'number'">
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :append-icon="field.passwordVisible ? 'visibility_off' : 'visibility'"
                    :append-icon-cb="appendPasswordIconCheckbox()"
                    :type="field.passwordVisible ? 'text' : 'password'"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            ></v-text-field>
        </div>


        <div v-else-if="field.type == 'checkbox'">
            <v-checkbox
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :disabled="field.disabled"
            ></v-checkbox>
        </div>


        <div v-else>
            <v-text-field
                    v-model="localValue"
                    :label="field.label"
                    :required="field.required"
                    :readonly="field.readonly"
                    :disabled="field.disabled"
                    :placeholder="field.placeholder"
                    :counter="field.counter"
                    :hint="field.hint"
                    @blur="onBlur"
                    @change="onChange"
                    @focus="onFocus"
                    @input="onInput"
            ></v-text-field>


            <v-alert v-if="field.type != 'text'" color="error" icon="warning" value="true">
                <strong>The {{field.type}} type is not yet implemented.</strong> <br>
                {{field}}
            </v-alert>

        </div>
    </div>
</template>

<script>
  export default {
    name: 'v-form-generator-field',
    props: {
      field: Object,
      value: null
    },
    data () {
      return {
        localValue: this.value,
        menu: false,
        dateFormatted: null,
        validationRules: {
          email: [
            (v) => /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(v) || this.validationErrorMessages.emailInvalid
          ]
        },
        validationErrorMessages: {
          'emailInvalid': 'E-mail must be valid'
        }
      }
    },
    created: function () {
      // On load
    },
    methods: {
      onBlur: function () {
        this.$emit('blur')
      },
      onChange: function () {
        this.$emit('change')
      },
      onFocus: function () {
        this.$emit('focus')
      },
      onInput: function () {
        this.$emit('input')
      },
      formatDate (date) {
        if (!date) {
          return null
        }
        const [year, month, day] = date.split('-')
        return `${month}/${day}/${year}`
      },
      parseDate (date) {
        if (!date) {
          return null
        }
        const [month, day, year] = date.split('/')
        return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
      },

      appendPasswordIconCheckbox () {
        return () => this.field.passwordVisible = !this.field.passwordVisible
      }
    }
  }
</script>
