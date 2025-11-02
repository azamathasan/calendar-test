<template>
   <div class="app-container">
    <div class="calendar-container">
      <div class="calendar-header">
        <div
          class="arrow left-arrow"
          @click="prevMonth"
        >&#9664;</div>
        <div class="current-month">
          <div class="month">{{ currentDate.toLocaleString('default', { month: 'short' }) }}</div>
          <div class="year">{{ currentDate.getFullYear()}}</div>
        </div>
        <div
          class="arrow right-arrow"
          @click="nextMonth"
        >&#9654;</div>
      </div>
      <div class="week-day-container">
        <div class="week-day">Sun</div>
        <div class="week-day">Mon</div>
        <div class="week-day">Tue</div>
        <div class="week-day">Wed</div>
        <div class="week-day">Thu</div>
        <div class="week-day">Fri</div>
        <div class="week-day">Sat</div>
      </div>
      <div class="calendar">
        <div
          class="day"
          v-for="day in firstDayOfMonth.getDay()"
          :key="day"
        ></div>
        <div
          :class="['day', activeDay==day?'active':'']"
          v-for="day in lastDayOfMonth"
          :key="day"
          @click="setDay(day)"
        >{{day}}</div>
      </div>
    </div>
    <div class="event-container">
      <input id="event" :value="currentDate.toJSON().slice(0,10)" type="textarea">
    </div>
   </div>
</template>

<script setup>
import { ref, computed } from 'vue';
const currentDate = ref(new Date());
const activeDay = computed(() => {
  return currentDate.value.getDate();
})
const firstDayOfMonth = computed(() => {
  return new Date(currentDate.value.getFullYear(), currentDate.value.getMonth(), 1);
});
const nextMonth = () => {
  currentDate.value = new Date(currentDate.value.setMonth(currentDate.value.getMonth() + 1));
}
const prevMonth = () => {
  currentDate.value = new Date(currentDate.value.setMonth(currentDate.value.getMonth() - 1));
}
const setDay = (day) => {
  currentDate.value = new Date(currentDate.value.setDate(day));
}
const lastDayOfMonth = computed(() => {
      const year = currentDate.value.getFullYear();
      const month = currentDate.value.getMonth();
      return new Date(year, month + 1, 0).getDate();
    });
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.app-container {
  display: flex;
  gap: 20px;
}

.calendar {
  width: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  &-container {
    width: 200px;
    border: 1px solid black;
    padding: 10px;
  }
  &-header {
    width: 100%;
    display: flex;
    justify-content: space-between;
    margin-bottom: 15px;
  }
}
.current-month {
  display: flex;
  gap: 5px;
}
.week-day {
  width: 28px;
  &-container {
    width: 100%;
    display: flex;
    font-size: 12px;
  }
  margin-bottom: 15px;
}
.arrow {
  cursor: pointer;
  width: 28px;
  height: 28px;
  line-height: 28px;
  &:hover {
    background-color: grey;
  }
}
.day {
  width: 26px;
  height: 26px;
  line-height: 26px;
  border: 1px solid white;
  cursor: pointer;
  &:hover {
    background-color: grey;
  }
  &.active {
    border-color: blue;
  }
}
.event {
  &-container {}
}
</style>
