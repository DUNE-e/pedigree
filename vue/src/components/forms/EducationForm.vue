<template>
  <div class="custom-form">
    <PopOver>
      <ElInput
        v-model="type"
        class="custom-form__input"
        type="text"
        placeholder="Тип"
      />

      <template slot="popover">
        <InputHelper 
          :options="hints" 
          @select="(hint) => selectHint(hint)"
        />
      </template>
    </PopOver>
    <ElInput
      v-model="level"
      class="custom-form__input"
      type="text"
      placeholder="Уровень"
    />
    <ElDatePicker
      v-model="startDate"
      class="custom-form__input"
      type="date"
      format="dd.MM.yyyy"
      value-format="dd.MM.yyyy"
      placeholder="Дата начала обучения"
      :picker-options="startPickerOptions"
    />
    <ElDatePicker
      v-model="endDate"
      class="custom-form__input"
      type="date"
      format="dd.MM.yyyy"
      value-format="dd.MM.yyyy"
      placeholder="Дата завершения обучения"
      :picker-options="endPickerOptions"
    />
    <ElInput
      v-model="name"
      class="custom-form__input"
      type="text"
      placeholder="Название учреждения"
    />
    <ElInput
      v-model="city"
      class="custom-form__input"
      type="text"
      placeholder="Город учреждения"
    />
  </div>
</template>

<script>
import PopOver from '../ui/PopOver.vue'
import InputHelper from '../ui/InputHelper.vue'

export default {
  name: 'EducationForm',
  components: {
    PopOver,
    InputHelper
  },
  model: {
    prop: 'value',
    event: 'change'
  },
  props: {
    value: {
      type: Object,
      required: true
    }
  },
  computed: {
    type: {
      get () {
        return this.value.type
      },
      set (value) {
        this.emitChange({ type: value })
      }
    },
    level: {
      get () {
        return this.value.level
      },
      set (value) {
        this.emitChange({ level: value })
      }
    },
    startDate: {
      get () {
        return this.value.startDate
      },
      set (value) {
        this.emitChange({ startDate: value })
      }
    },
    endDate: {
      get () {
        return this.value.endDate
      },
      set (value) {
        this.emitChange({ endDate: value })
      }
    },
    name: {
      get () {
        return this.value.name
      },
      set (value) {
        this.emitChange({ name: value })
      }
    },
    city: {
      get () {
        return this.value.city
      },
      set (value) {
        this.emitChange({ city: value })
      }
    },
    hints: {
      get() {
        return ['Бакалавриат', 'Магистратура', 'Аспирантура']
      }
    },
    startPickerOptions () {
      return {
        disabledDate: time => {
          if (this.endDate) {
            const endDate = this.parseDateString(this.endDate)
            return endDate && time.getTime() > endDate.getTime()
          }
        }
      }
    },
    endPickerOptions () {
      return {
        disabledDate: time => {
          if (this.startDate) {
            const startDate = this.parseDateString(this.startDate)
            return startDate && time.getTime() < startDate.getTime()
          }
        }
      }
    }
  },
  methods: {
    emitChange (param) {
      this.$emit('change', {
        ...this.value,
        ...param
      })
    },
    selectHint(hint) {
      this.type = hint
    },
    parseDateString (dateString) {
      if (!dateString) return null
      const parts = dateString.split(".")
      const day = parseInt(parts[0], 10)
      const month = parseInt(parts[1], 10) - 1
      const year = parseInt(parts[2], 10)
      return new Date(year, month, day)
    }
  }
}
</script>

<style>
</style>
