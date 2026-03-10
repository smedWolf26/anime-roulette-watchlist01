<script setup>
import { computed } from 'vue'
import AnimeCard from '@/components/AnimeCard.vue'
import { useAnimeRoulette } from '@/composables/useAnimeRoulette'

const { anime, loading, error, spin, cooldownLeft } = useAnimeRoulette()

const spinDisabled = computed(() => loading.value || cooldownLeft.value > 0)

const spinLabel = computed(() => {
  if (loading.value) {
    return 'Spinning...'
  }

  if (cooldownLeft.value > 0) {
    return `Cooldown ${cooldownLeft.value}`
  }

  return 'SPIN 🎰'
})
</script>

<template>
  <main
    class="min-h-screen bg-[radial-gradient(circle_at_15%_20%,#334155,transparent_40%),radial-gradient(circle_at_85%_0%,#0f766e,transparent_28%),linear-gradient(160deg,#020617,#0f172a,#111827)] px-4 py-8 text-slate-100 sm:px-6 lg:px-8"
  >
    <div class="mx-auto max-w-7xl">
      <header>
        <p class="text-xs font-semibold tracking-[0.3em] text-blue-400 uppercase">Project #4</p>
        <h1 class="mt-2 text-4xl font-black text-white sm:text-5xl">Anime Roulette Machine</h1>
        <p class="mt-2 max-w-3xl text-sm text-slate-300 sm:text-base">
          Spin the wheel, request a raondom anime from Jinkan with VueUse useFetch, and learn how
          REST APIs single rate lmiting with HTTP429
        </p>
      </header>

      <div class="grid gap-6 lg:grid-cols-[1.2fr_0.8fr]">
        <section class="space-y-5">
          <div
            class="rounded-3xl border border-slate-600/70 bg-slate-800/60 p-5 shadow-2xl shadow-slate-900/30 backdrop-blur-sm"
          >
            <div class="flex flex-col gap-3 sm:flex-row sm:items-center sm:justify-between">
              <div>
                <h2 class="text-xl font-bold text-white">Roulette</h2>
                <p class="text-sm text-slate-300">
                  Pull the lever for your next random anime recommendation.
                </p>
              </div>
              <button
                type="button"
                :disabled="spinDisabled"
                class="cursor-pointer rounded-full border-blue-300/70 bg-blue-400/20 px-6 py-3 text-base font-black tracking-wide text-blue-200 hover:bg-blue-400/30 disabled:cursor-not-allowed disabled:opacity-60"
                @click="spin"
              >
                {{ spinLabel }}
              </button>
            </div>
            <p
              v-if="cooldownLeft > 0"
              class="mt-4 rounded-xl border border-fuchsia-300/50 bg-fuchsia-400/10 px-3 py-2 text-sm font-semibold text-fuchsia-100"
            >
              Rate-limited. Try again in {{ cooldownLeft }}
            </p>
          </div>
          <AnimeCard
            :loading="loading"
            :error="error"
            :anime="anime"
          />
        </section>
      </div>
    </div>
  </main>
</template>
