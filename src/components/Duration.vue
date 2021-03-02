<template>
  <div>
    <input v-model.number="year" type="number" min="0" placeholder="year"/>
    <input v-model.number="month" type="number" min="0" max="12" placeholder="month"/>
    <input v-model.number="day" type="number" min="0" max="31" placeholder="day"/>
    <input v-model.number="hour" type="number" min="0" max="24" placeholder="hour"/>
    <input v-model.number="minute" type="number" min="0" max="60" placeholder="minute"/>
    <input v-model.number="second" type="number" min="0" max="60" placeholder="second"/>
    <br>
    Внутри модуля: {{ modelValue }}
  </div>
</template>

<script>
import {parse} from "iso8601-duration";

export default {
  name: 'Duration',
  props: {
    modelValue: {
      validator: function (val) {
        try {
          parse(val);
        } catch {
          return false;
        }
        return true;
      },
    }
  },
  data: function () {
    return {
      year: '',
      month: '',
      day: '',
      hour: '',
      minute: '',
      second: '',
    };
  },
  methods: {
    valid: function (a) {
      if (a != '' && a != 0) {
        return a;
      }
      return 0;
    },
    update: function () {
      this.year = this.parsed.years;
      this.month = this.parsed.months;
      this.day = this.parsed.days;
      this.hour = this.parsed.hours;
      this.minute = this.parsed.minutes;
      this.second = this.parsed.seconds;

      this.$emit('update:modelValue', this.state);
    },
  },
  computed: {
    state: function () {
      const year = this.valid(this.year),
          month = this.valid(this.month),
          day = this.valid(this.day),
          hour = this.valid(this.hour),
          minute = this.valid(this.minute),
          second = this.valid(this.second);
      let data = '', time = '';

      // Если переменная не задана - она не включается в строку
      if (year) {
        data += year + 'Y';
      }
      if (month) {
        data += month + 'M';
      }
      if (day) {
        data += day + 'D';
      }
      if (hour) {
        time += hour + 'H';
      }
      if (minute) {
        time += minute + 'M';
      }
      if (second) {
        time += second + 'S';
      }
      if (time != '') {
        time = 'T' + time;
      }
      // Если всё по нулям то заданным в 0
      if (data == '' && time == '') {
        return 'PT0S';
      }

      return 'P' + data + time;
    },
    parsed: function () {
      return parse(this.modelValue);
    },
  },
  watch: {
    state: function () {
      this.$emit('update:modelValue', this.state);
    },
    parsed: function () {
      this.update();
    },
  },
  created: function () {
    this.update();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
  width: 100px;
}
</style>
