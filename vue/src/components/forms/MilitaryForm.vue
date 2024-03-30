<template>
  <div class="custom-form">
    <ElInput
      v-model="type"
      class="custom-form__input"
      type="text"
      placeholder="Тип"
    />
    <ElInput
      v-model="rank"
      class="custom-form__input"
      type="text"
      placeholder="Звание, должность"
    />
    <ElDatePicker
      v-model="startDate"
      class="custom-form__input"
      type="date"
      format="dd.MM.yyyy"
      value-format="dd.MM.yyyy"
      placeholder="Дата начала службы"
      :picker-options="startPickerOptions"
    />
    <ElDatePicker
      v-model="endDate"
      class="custom-form__input"
      type="date"
      format="dd.MM.yyyy"
      value-format="dd.MM.yyyy"
      placeholder="Дата конца службы"
      :picker-options="endPickerOptions"
    />
    <div class="custom-form__full-width">
      <ElInput
        v-model="description"
        class="custom-form__input"
        type="textarea"
        placeholder="Описание"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'MilitaryForm',
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
      get() {
        return this.value.type
      },
      set(value) {
        this.emitChange({ type: value })
      }
    },
    rank: {
      get() {
        return this.value.rank
      },
      set(value) {
        this.emitChange({ rank: value })
      }
    },
    startDate: {
      get() {
        return this.value.startDate
      },
      set(value) {
        this.emitChange({ startDate: value })
      }
    },
    endDate: {
      get() {
        return this.value.endDate
      },
      set(value) {
        this.emitChange({ endDate: value })
      }
    },
    description: {
      get() {
        return this.value.description
      },
      set(value) {
        this.emitChange({ description: value })
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
 