<template>
  <div id="app">
    <TimePicker
      :service_id="1"
      :employee_id="123"
      :date="selectedDate"
      @update:datetime="handleDatetimeUpdate"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import TimePicker from './components/TimePicker.vue';

export default defineComponent({
  name: 'App',
  components: {
    TimePicker,
  },
  setup() {
    const selectedDate = ref<Date>(new Date());

    // Обработка события выбора времени из компонента
    const handleDatetimeUpdate = (slot: any) => {
      console.log('Выбранное время:', slot);
      // Можно сохранить slot в состояние или передать дальше
    };

    return {
      selectedDate,
      handleDatetimeUpdate,
    };
  },
});
</script>

<style>
/* Пример базовых стилей и объявления css-переменных для тем */
:root {
  --tg-theme-bg-color: #fff;
  --tg-theme-secondary-bg-color: #f3f2f8;
  --tg-theme-text-color: #111;
  --tg-theme-button-color: #59b7ff;
  --tg-theme-header-bg-color: #def1fd;
  --tg-theme-hint-color: #777;
}

body {
  margin: 0;
  font-family: sans-serif;
  background-color: var(--tg-theme-secondary-bg-color);
}
</style>
