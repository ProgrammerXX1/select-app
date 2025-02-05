<template>
  <div class="time-picker" :class="currentTheme">
    <!-- Кнопка выбора времени, на которой показывается выбранное время (если уже выбрано) -->
    <button class="select-time-button" @click="openModal">
      {{ selectedSlot ? formatSlot(selectedSlot) : 'Выбрать время' }}
    </button>

    <!-- Модальное окно -->
    <div v-if="isModalOpen" class="modal-overlay" @click.self="closeModal">
      <div class="modal-content">
        <h3>Выберите время</h3>

        <!-- Блок переключения темы -->
        <div class="theme-toggle">
          <span
            >Текущая тема:
            {{ currentTheme === 'day' ? 'Дневная' : 'Ночная' }}</span
          >
          <button class="toggle-theme-button" @click="toggleTheme">
            Переключить тему
          </button>
        </div>

        <!-- Список слотов -->
        <div class="slots-container">
          <button
            v-for="(slot, index) in slots"
            :key="index"
            class="slot-button"
            :class="{
              selected: selectedSlot && slot.start_at === selectedSlot.start_at,
              disabled: !slot.is_available,
            }"
            :disabled="!slot.is_available"
            @click="selectSlot(slot)"
          >
            {{ formatSlot(slot) }}
          </button>
        </div>

        <!-- Отображение выбранного времени до нажатия "Готово" -->
        <div v-if="selectedSlot" class="selected-display">
          Вы выбрали: {{ formatSlot(selectedSlot) }}
        </div>

        <!-- Кнопка подтверждения выбора -->
        <button
          class="done-button"
          :disabled="!selectedSlot"
          @click="confirmSelection"
        >
          Готово
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { parseISO, format } from 'date-fns';

interface TimeSlot {
  start_at: string;
  finish_at: string;
  is_available: boolean;
}

export default defineComponent({
  name: 'TimePicker',
  props: {
    service_id: {
      type: Number,
      required: true,
    },
    employee_id: {
      type: Number,
      required: true,
    },
    date: {
      type: Date,
      required: true,
    },
  },
  emits: ['update:datetime'],
  setup(props, { emit }) {
    // Флаг отображения модального окна
    const isModalOpen = ref<boolean>(false);
    // Список временных слотов (имитация получения по API)
    const slots = ref<TimeSlot[]>([]);
    // Выбранный слот
    const selectedSlot = ref<TimeSlot | null>(null);
    // Текущая тема: "day" или "night" (по умолчанию дневная)
    const currentTheme = ref<'day' | 'night'>('day');

    // Открытие модального окна и загрузка слотов
    const openModal = (): void => {
      isModalOpen.value = true;
      fetchSlots();
    };

    // Закрытие модального окна
    const closeModal = (): void => {
      isModalOpen.value = false;
    };

    // Выбор слота (если доступен)
    const selectSlot = (slot: TimeSlot): void => {
      if (!slot.is_available) return;
      selectedSlot.value = slot;
    };

    // Подтверждение выбора: эмитируем выбранное время и закрываем модалку
    const confirmSelection = (): void => {
      if (selectedSlot.value) {
        emit('update:datetime', selectedSlot.value);
      }
      closeModal();
    };

    // Имитируем запрос к API для получения слотов
    const fetchSlots = async (): Promise<void> => {
      const simulatedResponse: TimeSlot[] = [
        {
          start_at: '2025-01-18T07:00:00Z',
          finish_at: '2025-01-18T07:40:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T07:40:00Z',
          finish_at: '2025-01-18T08:20:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T08:20:00Z',
          finish_at: '2025-01-18T09:00:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T09:00:00Z',
          finish_at: '2025-01-18T09:40:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T09:40:00Z',
          finish_at: '2025-01-18T10:20:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T10:20:00Z',
          finish_at: '2025-01-18T11:00:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T11:00:00Z',
          finish_at: '2025-01-18T11:40:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T11:40:00Z',
          finish_at: '2025-01-18T12:20:00Z',
          is_available: false,
        },
        {
          start_at: '2025-01-18T12:20:00Z',
          finish_at: '2025-01-18T13:00:00Z',
          is_available: false,
        },
        {
          start_at: '2025-01-18T13:00:00Z',
          finish_at: '2025-01-18T13:40:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T13:40:00Z',
          finish_at: '2025-01-18T14:20:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T14:20:00Z',
          finish_at: '2025-01-18T15:00:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T15:00:00Z',
          finish_at: '2025-01-18T15:40:00Z',
          is_available: false,
        },
        {
          start_at: '2025-01-18T15:40:00Z',
          finish_at: '2025-01-18T16:20:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T16:20:00Z',
          finish_at: '2025-01-18T17:00:00Z',
          is_available: true,
        },
        {
          start_at: '2025-01-18T17:00:00Z',
          finish_at: '2025-01-18T17:40:00Z',
          is_available: false,
        },
        {
          start_at: '2025-01-18T17:40:00Z',
          finish_at: '2025-01-18T18:20:00Z',
          is_available: false,
        },
        {
          start_at: '2025-01-18T18:20:00Z',
          finish_at: '2025-01-18T19:00:00Z',
          is_available: false,
        },
        {
          start_at: '2025-01-18T19:00:00Z',
          finish_at: '2025-01-18T19:40:00Z',
          is_available: true,
        },
      ];
      // Имитируем задержку ответа
      await new Promise((resolve) => setTimeout(resolve, 500));
      slots.value = simulatedResponse;
    };

    // Форматирование слота: перевод времени из UTC в локальное, формат "HH:mm - HH:mm"
    const formatSlot = (slot: TimeSlot): string => {
      const startDate: Date = parseISO(slot.start_at);
      const finishDate: Date = parseISO(slot.finish_at);
      return `${format(startDate, 'HH:mm')} - ${format(finishDate, 'HH:mm')}`;
    };

    // Переключение темы
    const toggleTheme = (): void => {
      currentTheme.value = currentTheme.value === 'day' ? 'night' : 'day';
    };

    return {
      isModalOpen,
      slots,
      selectedSlot,
      currentTheme,
      openModal,
      closeModal,
      selectSlot,
      confirmSelection,
      formatSlot,
      toggleTheme,
    };
  },
});
</script>

<style scoped>
/* Корневой контейнер компонента */
.time-picker {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1rem;
}

/* Стили для блока переключения темы */
.theme-toggle {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  color: var(--tg-theme-text-color);
}

.theme-toggle span {
  font-weight: bold;
}

.toggle-theme-button {
  background-color: var(--tg-theme-button-color);
  color: #fff;
  border: none;
  padding: 0.4rem 0.8rem;
  border-radius: 4px;
  font-size: 0.8rem;
  cursor: pointer;
}

/* Кнопка выбора времени */
.select-time-button {
  background-color: var(--tg-theme-secondary-bg-color);
  color: var(--tg-theme-text-color);
  border: none;
  padding: 0.75rem 1rem;
  border-radius: 4px;
  width: 100%;
  max-width: 300px;
  font-size: 1rem;
}

/* Оверлей модального окна */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

/* Контент модального окна */
.modal-content {
  background-color: var(--tg-theme-bg-color);
  border-radius: 8px;
  padding: 1rem;
  width: 90%;
  max-width: 400px;
}

/* Заголовок модального окна */
.modal-content h3 {
  background-color: var(--tg-theme-header-bg-color);
  padding: 0.5rem;
  border-radius: 4px;
  text-align: center;
  margin-bottom: 1rem;
  color: var(--tg-theme-text-color);
}

/* Контейнер для слотов */
.slots-container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  justify-content: center;
}

/* Отображение выбранного времени до нажатия "Готово" */
.selected-display {
  margin: 1rem 0;
  text-align: center;
  font-weight: bold;
  color: var(--tg-theme-text-color);
}

/* Кнопки слотов */
.slot-button {
  flex: 1 1 30%;
  padding: 0.5rem;
  border: 1px solid var(--tg-theme-text-color);
  border-radius: 4px;
  background-color: transparent;
  color: var(--tg-theme-text-color);
  font-size: 0.9rem;
}

/* Стиль недоступного слота */
.slot-button.disabled {
  color: var(--tg-theme-hint-color);
  border-color: var(--tg-theme-hint-color);
}

/* Стиль выбранного слота */
.slot-button.selected {
  background-color: var(--tg-theme-button-color);
  color: #fff;
  border-color: var(--tg-theme-button-color);
}

/* Кнопка подтверждения выбора */
.done-button {
  margin-top: 1rem;
  width: 100%;
  padding: 0.75rem;
  border: none;
  border-radius: 4px;
  background-color: var(--tg-theme-button-color);
  color: #fff;
  font-size: 1rem;
}

.done-button:disabled {
  background-color: var(--tg-theme-hint-color);
  cursor: not-allowed;
}

/* Mobile-first адаптация + медиа-запрос для больших экранов */
@media (min-width: 768px) {
  .modal-content {
    padding: 2rem;
  }
}

/* Тематические стили */
/* Дневная тема — можно использовать глобальные переменные (они заданы в корне приложения) */
.time-picker.day {
  /* Дополнительных переопределений не требуется */
}

/* Ночная тема: переопределяем CSS-переменные для улучшения читаемости */
.time-picker.night {
  --tg-theme-bg-color: #111;
  --tg-theme-secondary-bg-color: #222;
  --tg-theme-text-color: #fff;
  --tg-theme-button-color: #3b82f6;
  --tg-theme-header-bg-color: #1f2937;
  --tg-theme-hint-color: #ccc;
}
</style>
