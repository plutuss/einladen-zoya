<script setup>
import { computed, ref } from 'vue'

const showForm = ref(false)
const isSubmitted = ref(false)
const isNoButtonEscaped = ref(false)
const isMusicEnabled = ref(false)

const selectedDate = ref('')
const selectedTime = ref('')
const selectedPlan = ref('')

const noButtonStyle = ref({})


const youtubeUrl = ref('https://music.youtube.com/watch?v=MMcNLukFnE4&list=PLGIfL-J3DAh8F1qOzzJuU8zbJzJGdP9Sr')
const girlName = ref('Зоя')

const greetingText = computed(() => {
  return girlName.value
      ? `Привіт, ${girlName.value} 💕`
      : 'Привіт 💕'
})


const musicStart = ref(18)
const musicEnd = ref(360)

const plans = [
  {
    value: 'Кава та прогулянка',
    label: '☕ Кава та прогулянка',
    description: 'Легка зустріч, розмови та гарний настрій',
  },
  {
    value: 'Вечеря',
    label: '🍝 Вечеря',
    description: 'Смачний вечір у затишному місці',
  },
  {
    value: 'Кіно',
    label: '🎬 Кіно',
    description: 'Фільм, попкорн і трохи романтики',
  },
  {
    value: 'Вечірня прогулянка',
    label: '🌆 Вечірня прогулянка',
    description: 'Гарний вечір, місто і ми',
  },
  {
    value: 'Сюрприз від мене',
    label: '🎁 Сюрприз від мене',
    description: 'Я все підготую сам',
  },
]

const petals = Array.from({ length: 30 }, (_, index) => ({
  id: index,
  left: Math.floor(Math.random() * 100),
  delay: Math.random() * 8,
  duration: 8 + Math.random() * 10,
  size: 14 + Math.random() * 18,
  opacity: 0.35 + Math.random() * 0.55,
}))

const hearts = Array.from({ length: 12 }, (_, index) => ({
  id: index,
  left: Math.floor(Math.random() * 100),
  top: Math.floor(Math.random() * 100),
  delay: Math.random() * 4,
}))

const selectedPlanData = computed(() => {
  return plans.find((plan) => plan.value === selectedPlan.value)
})

const isFormValid = computed(() => {
  return selectedDate.value && selectedTime.value && selectedPlan.value
})

const youtubeVideoId = computed(() => {
  const url = youtubeUrl.value.trim()

  if (!url) {
    return null
  }

  const patterns = [
    /youtube\.com\/watch\?v=([^&]+)/,
    /youtu\.be\/([^?&]+)/,
    /youtube\.com\/embed\/([^?&]+)/,
    /youtube\.com\/shorts\/([^?&]+)/,
  ]

  for (const pattern of patterns) {
    const match = url.match(pattern)

    if (match?.[1]) {
      return match[1]
    }
  }

  return null
})

const youtubeEmbedUrl = computed(() => {
  if (!youtubeVideoId.value) {
    return null
  }

  const params = new URLSearchParams({
    autoplay: '1',
    controls: '0',
    rel: '0',
    start: String(musicStart.value),
    end: String(musicEnd.value),
    playsinline: '1',
  })

  return `https://www.youtube.com/embed/${youtubeVideoId.value}?${params.toString()}`
})

const enableMusic = () => {
  isMusicEnabled.value = true
}

const moveNoButton = () => {

  isNoButtonEscaped.value = true

  const buttonWidth = window.innerWidth < 640 ? 220 : 280

  const buttonHeight = 76

  const padding = 20

  const maxX = window.innerWidth - buttonWidth - padding

  const maxY = window.innerHeight - buttonHeight - padding

  noButtonStyle.value = {

    left: `${Math.max(padding, Math.random() * maxX)}px`,

    top: `${Math.max(padding, Math.random() * maxY)}px`,

    zIndex: 99999,

  }

}

const acceptInvite = () => {
  showForm.value = true
}

const selectPlan = (plan) => {
  selectedPlan.value = plan.value
}

const submitAnswer = async () => {
  if (!isFormValid.value) {
    return
  }

  isSubmitted.value = true

  console.log({
    date: selectedDate.value,
    time: selectedTime.value,
    plan: selectedPlan.value,
  })
}
</script>

<template>
  <div class="relative min-h-screen overflow-hidden bg-gradient-to-br w-screen from-rose-100 via-pink-100 to-purple-100 px-3 py-5 sm:px-4 sm:py-8">
    <!-- Hidden YouTube music player -->
    <iframe
        v-if="isMusicEnabled && youtubeEmbedUrl"
        :src="youtubeEmbedUrl"
        class="absolute h-0 w-0 opacity-0"
        allow="autoplay; encrypted-media"
        title="Romantic background music"
    ></iframe>

    <!-- Background glow -->
    <div class="pointer-events-none absolute -left-32 -top-32 h-80 w-80 rounded-full bg-pink-300/40 blur-3xl"></div>
    <div class="pointer-events-none absolute -right-32 top-24 h-96 w-96 rounded-full bg-purple-300/40 blur-3xl"></div>
    <div class="pointer-events-none absolute bottom-0 left-1/2 h-80 w-80 -translate-x-1/2 rounded-full bg-rose-300/30 blur-3xl"></div>

    <!-- Falling petals -->
    <div class="pointer-events-none absolute inset-0 overflow-hidden">
      <span
          v-for="petal in petals"
          :key="petal.id"
          class="petal"
          :style="{
          left: `${petal.left}%`,
          width: `${petal.size}px`,
          height: `${petal.size * 1.35}px`,
          animationDelay: `${petal.delay}s`,
          animationDuration: `${petal.duration}s`,
          opacity: petal.opacity,
        }"
      ></span>
    </div>

    <!-- Floating hearts -->
    <div class="pointer-events-none absolute inset-0 overflow-hidden">
      <span
          v-for="heart in hearts"
          :key="heart.id"
          class="floating-heart"
          :style="{
          left: `${heart.left}%`,
          top: `${heart.top}%`,
          animationDelay: `${heart.delay}s`,
        }"
      >
        ❤
      </span>
    </div>

    <div class="relative z-10 flex min-h-[calc(100vh-2.5rem)] items-center justify-center sm:min-h-[calc(100vh-4rem)]">
      <div class="w-full max-w-xl">
        <div class="relative overflow-hidden rounded-[1.75rem] border border-white/70 bg-white/75 p-5 shadow-2xl shadow-pink-200/70 backdrop-blur-xl sm:rounded-[2.5rem] sm:p-8 md:p-10">
          <!-- Decorative circles -->
          <div class="absolute -right-10 -top-10 h-32 w-32 rounded-full bg-pink-300/30"></div>
          <div class="absolute -bottom-12 -left-12 h-36 w-36 rounded-full bg-purple-300/20"></div>

          <template v-if="!showForm && !isSubmitted">
            <div class="relative text-center">
              <div class="heartbeat mx-auto mb-5 flex h-20 w-20 items-center justify-center rounded-full bg-gradient-to-br from-pink-400 to-rose-500 text-4xl shadow-lg shadow-pink-300/60 sm:h-24 sm:w-24 sm:text-5xl">
                💌
              </div>

              <p class="mb-3 text-sm font-bold uppercase tracking-[0.22em] text-pink-500 sm:text-base">

                маленьке запрошення

              </p>

              <p class="mb-3 text-2xl font-extrabold text-pink-600 sm:text-3xl">

                {{ greetingText }}

              </p>

              <h1 class="mb-5 text-4xl font-extrabold leading-tight tracking-tight text-gray-900 sm:text-5xl md:text-6xl">

                Підеш зі мною на побачення?

              </h1>

              <p class="mx-auto mb-7 max-w-md text-lg leading-8 text-gray-700 sm:text-xl">
                Хочу провести з тобою гарний вечір.
                Ти обираєш день, час і настрій, а я постараюся зробити все затишно.
              </p>

              <button
                  v-if="!isMusicEnabled"
                  type="button"
                  @click="enableMusic"
                  class="group mb-8 inline-flex items-center justify-center gap-3 rounded-3xl border border-pink-100 bg-white/95 px-8 py-6 text-base font-extrabold text-pink-600 shadow-xl shadow-pink-100/80 ring-1 ring-white/80 transition duration-300 hover:-translate-y-1 hover:bg-white hover:shadow-2xl active:scale-95 sm:text-lg"
              >
                <span class="flex h-9 w-9 items-center justify-center rounded-full bg-pink-100 text-lg transition group-hover:bg-pink-200">
                  ▶
                </span>
                Увімкнути музику
              </button>

              <div
                  v-else
                  class="mx-auto mb-8 max-w-xs rounded-2xl bg-pink-50/90 px-4 py-3 text-sm font-bold text-pink-600 ring-1 ring-pink-100"
              >
                🎵 Музика вже грає
              </div>

<!--              <div class="relative mt-2 flex flex-col items-center justify-center gap-4 sm:flex-row">-->
<!--                <button-->
<!--                    @click="acceptInvite"-->
<!--                    class="group relative min-h-16 w-full overflow-hidden rounded-3xl bg-gradient-to-r from-pink-500 via-rose-500 to-fuchsia-500 px-12 py-5 text-xl font-extrabold text-white shadow-xl shadow-pink-300/70 transition duration-300 hover:-translate-y-1 hover:scale-105 hover:shadow-2xl active:scale-95 sm:w-auto sm:min-w-48"-->
<!--                >-->
<!--                  <span class="relative z-10 flex items-center justify-center gap-3">-->
<!--                    Так-->
<!--                    <span class="text-3xl">💖</span>-->
<!--                  </span>-->

<!--                  <span class="absolute inset-0 -translate-x-full bg-white/25 transition duration-500 group-hover:translate-x-0"></span>-->
<!--                  <span class="absolute -right-6 -top-6 h-16 w-16 rounded-full bg-white/20 blur-xl"></span>-->
<!--                </button>-->

<!--                  <button-->
<!--                      v-if="!isNoButtonEscaped"-->
<!--                      @mouseenter="moveNoButton"-->
<!--                      @touchstart.prevent="moveNoButton"-->
<!--                      :style="noButtonStyle"-->
<!--                      class="fixed min-h-16 rounded-3xl border border-pink-100 bg-white/95 px-8 py-5 text-lg font-extrabold text-gray-700 shadow-2xl shadow-pink-200/80 ring-1 ring-white/80 transition duration-300 hover:-translate-y-1 hover:bg-white active:scale-95"-->
<!--                  >-->
<!--                    Ні, я поки соромлюся 🙈-->
<!--                  </button>-->

<!--              </div>-->
              <div class="relative mt-2 flex flex-col items-center justify-center gap-4 sm:flex-row">

                <button

                    @click="acceptInvite"

                    class="group relative min-h-16 w-full overflow-hidden rounded-3xl bg-gradient-to-r from-pink-500 via-rose-500 to-fuchsia-500 px-12 py-5 text-xl font-extrabold text-white shadow-xl shadow-pink-300/70 transition duration-300 hover:-translate-y-1 hover:scale-105 hover:shadow-2xl active:scale-95 sm:w-auto sm:min-w-48"

                >

    <span class="relative z-10 flex items-center justify-center gap-3">

      Так

      <span class="text-3xl">💖</span>

    </span>

                  <span class="absolute inset-0 -translate-x-full bg-white/25 transition duration-500 group-hover:translate-x-0"></span>

                  <span class="absolute -right-6 -top-6 h-16 w-16 rounded-full bg-white/20 blur-xl"></span>

                </button>

                <button

                    v-if="!isNoButtonEscaped"

                    @mouseenter="moveNoButton"

                    @touchstart.prevent="moveNoButton"

                    type="button"

                    class="min-h-16 w-full rounded-3xl border border-pink-100 bg-white/95 px-8 py-5 text-lg font-extrabold text-gray-700 shadow-xl shadow-pink-100/80 ring-1 ring-white/80 transition duration-300 hover:-translate-y-1 hover:bg-white hover:shadow-2xl active:scale-95 sm:w-auto sm:min-w-64"

                >

                  Ні, я поки соромлюся 🙈

                </button>

              </div>
              <p class="mt-10 text-sm font-medium text-gray-500">
                Підказка: правильна кнопка рожева 😄
              </p>
            </div>
          </template>

          <template v-else-if="showForm && !isSubmitted">
            <div class="relative">
              <div class="mb-7 text-center">
                <div class="mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-pink-100 text-3xl shadow-inner sm:h-20 sm:w-20 sm:text-4xl">
                  🌹
                </div>

                <h2 class="mb-3 text-3xl font-extrabold text-gray-900 sm:text-4xl">
                  Ура! Тоді обирай
                </h2>

                <p class="text-base text-gray-600 sm:text-lg">
                  Я вже радію цьому вечору 😊
                </p>
              </div>

              <div class="space-y-5">
                <div>
                  <label class="mb-2 block text-base font-extrabold text-gray-700">
                    📅 Дата
                  </label>
                  <input
                      v-model="selectedDate"
                      type="date"
                      class="w-full rounded-2xl border border-pink-100 bg-white/90 px-4 py-4 text-base text-gray-800 shadow-sm outline-none transition focus:border-pink-400 focus:ring-4 focus:ring-pink-100 sm:text-lg"
                  />
                </div>

                <div>
                  <label class="mb-2 block text-base font-extrabold text-gray-700">
                    🕒 Час
                  </label>
                  <input
                      v-model="selectedTime"
                      type="time"
                      class="w-full rounded-2xl border border-pink-100 bg-white/90 px-4 py-4 text-base text-gray-800 shadow-sm outline-none transition focus:border-pink-400 focus:ring-4 focus:ring-pink-100 sm:text-lg"
                  />
                </div>

                <div>
                  <label class="mb-3 block text-base font-extrabold text-gray-700">
                    ✨ Що обираєш?
                  </label>

                  <div class="grid gap-3">
                    <button
                        v-for="plan in plans"
                        :key="plan.value"
                        type="button"
                        @click="selectPlan(plan)"
                        class="rounded-3xl border p-4 text-left transition duration-300 hover:-translate-y-0.5 hover:shadow-lg sm:p-5"
                        :class="selectedPlan === plan.value
                        ? 'border-pink-400 bg-pink-50 shadow-lg shadow-pink-100'
                        : 'border-pink-100 bg-white/75 hover:border-pink-300'"
                    >
                      <div class="flex items-start justify-between gap-4">
                        <div>
                          <p class="text-base font-extrabold text-gray-900 sm:text-lg">
                            {{ plan.label }}
                          </p>
                          <p class="mt-1 text-sm leading-6 text-gray-500 sm:text-base">
                            {{ plan.description }}
                          </p>
                        </div>

                        <span
                            class="flex h-8 w-8 shrink-0 items-center justify-center rounded-full border text-sm font-extrabold"
                            :class="selectedPlan === plan.value
                            ? 'border-pink-500 bg-pink-500 text-white'
                            : 'border-gray-300 text-transparent'"
                        >
                          ✓
                        </span>
                      </div>
                    </button>
                  </div>
                </div>

                <div
                    v-if="selectedPlanData"
                    class="rounded-2xl bg-gradient-to-r from-pink-50 to-rose-50 p-4 text-base text-gray-700 ring-1 ring-pink-100"
                >
                  <strong>Обраний варіант:</strong>
                  {{ selectedPlanData.label }}
                </div>

                <button
                    @click="submitAnswer"
                    :disabled="!isFormValid"
                    class="group relative w-full overflow-hidden rounded-3xl bg-gradient-to-r from-pink-500 via-rose-500 to-fuchsia-500 py-5 text-xl font-extrabold text-white shadow-xl shadow-pink-300/60 transition duration-300 hover:-translate-y-1 hover:scale-[1.02] hover:shadow-2xl active:scale-95 disabled:cursor-not-allowed disabled:opacity-50 disabled:hover:translate-y-0 disabled:hover:scale-100"
                >
                  <span class="relative z-10">
                    Надіслати відповідь 💌
                  </span>

                  <span class="absolute inset-0 -translate-x-full bg-white/25 transition duration-500 group-hover:translate-x-0"></span>
                </button>
              </div>
            </div>
          </template>

          <template v-else>
            <div class="relative text-center">
              <div class="heartbeat mx-auto mb-5 flex h-24 w-24 items-center justify-center rounded-full bg-gradient-to-br from-pink-400 to-rose-500 text-5xl shadow-xl shadow-pink-300/60">
                💖
              </div>

              <h2 class="mb-4 text-4xl font-extrabold text-gray-900 sm:text-5xl">
                Чудово!
              </h2>

              <p class="mb-6 text-lg leading-8 text-gray-700">
                Я отримав твій вибір. Тепер залишилося тільки дочекатися нашого вечора 😊
              </p>

              <div class="rounded-3xl bg-white/85 p-5 text-left text-base shadow-inner ring-1 ring-pink-100 sm:text-lg">
                <p class="mb-3 text-sm text-gray-500">
                  Твій вибір:
                </p>

                <div class="space-y-2 text-gray-800">
                  <p>
                    <strong>📅 Дата:</strong> {{ selectedDate }}
                  </p>
                  <p>
                    <strong>🕒 Час:</strong> {{ selectedTime }}
                  </p>
                  <p>
                    <strong>✨ Варіант:</strong> {{ selectedPlan }}
                  </p>
                </div>
              </div>

              <p class="mt-6 text-base font-bold text-pink-500">
                До зустрічі 🌹
              </p>
            </div>
          </template>
        </div>
      </div>
    </div>
  </div>

  <Teleport to="body">

    <button

        v-if="isNoButtonEscaped"

        @mouseenter="moveNoButton"

        @touchstart.prevent="moveNoButton"

        :style="noButtonStyle"

        class="fixed min-h-16 rounded-3xl border border-pink-100 bg-white/95 px-8 py-5 text-lg font-extrabold text-gray-700 shadow-2xl shadow-pink-200/80 ring-1 ring-white/80 transition duration-300 hover:-translate-y-1 hover:bg-white active:scale-95"

    >

      Ні, я поки соромлюся 🙈

    </button>

  </Teleport>
</template>

<style scoped>
.petal {
  position: absolute;
  top: -40px;
  display: block;
  border-radius: 80% 0 80% 0;
  background: linear-gradient(135deg, rgba(244, 114, 182, 0.95), rgba(251, 207, 232, 0.85));
  animation-name: fall;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  transform: rotate(45deg);
}

.petal::after {
  content: '';
  position: absolute;
  inset: 20%;
  border-radius: inherit;
  background: rgba(255, 255, 255, 0.25);
}

.floating-heart {
  position: absolute;
  color: rgba(244, 114, 182, 0.25);
  font-size: 22px;
  animation: floatHeart 5s ease-in-out infinite;
}

.heartbeat {
  animation: heartbeat 1.8s ease-in-out infinite;
}

@keyframes fall {
  0% {
    transform: translate3d(0, -60px, 0) rotate(0deg);
  }

  50% {
    transform: translate3d(60px, 50vh, 0) rotate(180deg);
  }

  100% {
    transform: translate3d(-40px, 110vh, 0) rotate(360deg);
  }
}

@keyframes floatHeart {
  0%,
  100% {
    transform: translateY(0) scale(1);
  }

  50% {
    transform: translateY(-18px) scale(1.15);
  }
}

@keyframes heartbeat {
  0%,
  100% {
    transform: scale(1);
  }

  14% {
    transform: scale(1.08);
  }

  28% {
    transform: scale(1);
  }

  42% {
    transform: scale(1.08);
  }

  70% {
    transform: scale(1);
  }
}

@media (max-width: 640px) {
  .petal {
    animation-duration: 12s;
  }

  .floating-heart {
    font-size: 18px;
  }
}
</style>