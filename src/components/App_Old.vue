<script setup>
import { ref } from 'vue'

const showForm = ref(false)

const noButtonStyle = ref({
  position: 'absolute',
  left: '50%',
  top: '65%',
})

const selectedDate = ref('')
const selectedTime = ref('')
const selectedPlan = ref('')

// const submitAnswer = async () => {
//   const token = 'BOT_TOKEN'
//   const chatId = 'YOUR_CHAT_ID'
//
//   const message = `
// 💌 Новое приглашение принято!
//
// Дата: ${selectedDate.value}
// Время: ${selectedTime.value}
// Вариант: ${selectedPlan.value}
//     `
//
//   await fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
//     method: 'POST',
//     headers: {
//       'Content-Type': 'application/json',
//     },
//     body: JSON.stringify({
//       chat_id: chatId,
//       text: message,
//     }),
//   })
//
//   alert('Ответ отправлен 😊')
// }

const plans = [
  'Кофе и прогулка',
  'Ужин',
  'Кино',
  'Прогулка вечером',
  'Сюрприз от меня',
]

const moveNoButton = () => {
  const maxX = window.innerWidth - 120
  const maxY = window.innerHeight - 60

  noButtonStyle.value = {
    position: 'absolute',
    left: `${Math.random() * maxX}px`,
    top: `${Math.random() * maxY}px`,
  }
}

const acceptInvite = () => {
  showForm.value = true
}

const submitAnswer = () => {
  alert(`Супер! 😊\nДата: ${selectedDate.value}\nВремя: ${selectedTime.value}\nПлан: ${selectedPlan.value}`)
}
</script>

<template>
  <div class="min-h-screen bg-pink-50 flex items-center justify-center px-4 relative overflow-hidden">
    <div class="bg-white rounded-3xl shadow-xl p-8 max-w-md w-full text-center">
      <template v-if="!showForm">
        <h1 class="text-3xl font-bold text-pink-600 mb-4">
          Приглашение на свидание 💌
        </h1>

        <p class="text-gray-700 mb-8">
          Хочу провести с тобой хороший вечер. Согласна выбрать день и место? 😊
        </p>

        <div class="flex justify-center gap-4">
          <button
              @click="acceptInvite"
              class="px-6 py-3 rounded-xl bg-pink-500 text-white font-semibold hover:bg-pink-600 transition"
          >
            Да 💖
          </button>

          <button
              @mouseenter="moveNoButton"
              @touchstart.prevent="moveNoButton"
              :style="noButtonStyle"
              class="px-6 py-3 rounded-xl bg-gray-200 text-gray-700 font-semibold transition"
          >
            Нет, я пока стесняюсь 🙈
          </button>
        </div>
      </template>

      <template v-else>
        <h2 class="text-2xl font-bold text-pink-600 mb-4">
          Ура! Тогда выбери вариант 😊
        </h2>

        <div class="space-y-4 text-left">
          <div>
            <label class="block text-sm font-medium mb-1">
              Дата
            </label>
            <input
                v-model="selectedDate"
                type="date"
                class="w-full border rounded-xl px-4 py-2"
            />
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">
              Время
            </label>
            <input
                v-model="selectedTime"
                type="time"
                class="w-full border rounded-xl px-4 py-2"
            />
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">
              Что выбираешь?
            </label>
            <select
                v-model="selectedPlan"
                class="w-full border rounded-xl px-4 py-2"
            >
              <option value="" disabled>
                Выбери вариант
              </option>
              <option
                  v-for="plan in plans"
                  :key="plan"
                  :value="plan"
              >
                {{ plan }}
              </option>
            </select>
          </div>

          <button
              @click="submitAnswer"
              :disabled="!selectedDate || !selectedTime || !selectedPlan"
              class="w-full py-3 rounded-xl bg-pink-500 text-white font-semibold hover:bg-pink-600 disabled:opacity-50 disabled:cursor-not-allowed transition"
          >
            Отправить ответ 💌
          </button>
        </div>
      </template>
    </div>
  </div>
</template>