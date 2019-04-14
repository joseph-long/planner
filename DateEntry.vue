<template>
    <div>
        <div>{{ value.year }}-{{ value.month }}-{{ value.day }}</div>
        <div>
            <input
                class="year"
                @input="update($event, 'year')"
                v-bind:value="yearInput"
            >-<input
                class="month"
                @input="update($event, 'month')"
                v-bind:value="monthInput"
            >-<input
                class="day"
                @input="update($event, 'day')"
                v-bind:value="dayInput">
        </div>
    </div>
</template>
<style scoped>
input {
    font-size: 1em;
}
input.year {
    width: 3em;
}
input.month, input.day {
    width: 2em;
}
</style>

<script>
import { DateTime } from 'luxon';

export default {
    props: ["value", "timezone"],
    methods: {
        update: function($event, which) {
            if (which === "year") {
                this.yearInput = $event.target.value;
            } else if (which === "month") {
                this.monthInput = $event.target.value;
            } else if (which === "day") {
                this.dayInput = $event.target.value;
            }
            let parsedDate = DateTime.local(Number(this.yearInput), Number(this.monthInput), Number(this.dayInput));
            parsedDate = parsedDate.setZone(this.timezone);
            if (parsedDate.isValid && parsedDate.year > 0) {
                this.$emit('input', parsedDate);
            }
        }
    },
    watch: {
        value: function (newValue) {
            this.yearInput = newValue.year;
            this.monthInput = newValue.month;
            this.dayInput = newValue.day;
        },
        timezone: function (newTimezone) {
            this.$emit('input', this.value.setZone(newTimezone));
        }
    },
    data: function () {
        return {
            yearInput: this.value.year,
            monthInput: this.value.month,
            dayInput: this.value.day
        }
    }
}
</script>
