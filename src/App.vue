<template>
  <div v-if="step === 14">
    <RouterView @stop-audio="() => myAudio.pause()" />
  </div>
  <div
    v-else
    class="fixed w-screen h-screen text-[#fff] bg-[#000] flex justify-center items-center text-[1.5rem] md:text-[2.5rem] sm:text-[2rem] lg:text-[3rem] xl:text-[3.5rem] 2xl:text-[5rem]"
  >
    <div v-if="!isStart">
      <motion.img
        layout
        class="absolute"
        v-for="(sakura, index) in sakuras"
        :key="index"
        :src="sakura.backgroundImage"
        :style="sakura.style"
        :initial="{ opacity: 0, top: '-1vh' }"
        :animate="{
          opacity: 1,
          x: [
            0,
            (100 / sakura.duration) * (sakura.rand ? 1 : -1),
            (200 / sakura.duration) * (sakura.rand ? 1 : -1),
            (100 / sakura.duration) * (sakura.rand ? 1 : -1),
            0,
            (-100 / sakura.duration) * (sakura.rand ? 1 : -1),
            (-200 / sakura.duration) * (sakura.rand ? 1 : -1),
            (-100 / sakura.duration) * (sakura.rand ? 1 : -1),
            0,
          ],
          top: '101vh',
          transition: {
            duration: sakura.duration + 1,
            repeat: Infinity,
            ease: 'linear',
            x: {
              duration: Math.min(10, Math.random() * 1000),
              repeat: Infinity,
            },
            delay: index * 0.1,
          },
        }"
      />
    </div>
    <div v-if="!isStart">
      <label for="ourday" class="block text-sm/8 font-medium">비밀번호 (우리가 만난 날)</label>
      <div class="mt-2">
        <div
          class="flex items-center rounded-md bg-white pl-3 outline-1 -outline-offset-1 outline-gray-300 has-[input:focus-within]:outline-1 has-[input:focus-within]:-outline-offset-1 has-[input:focus-within]:outline-[#68b0ed]"
        >
          <input
            type="text"
            name="ourday"
            id="ourday"
            v-model="password"
            class="block min-w-0 grow py-1 pr-3 text-base text-gray-900 placeholder:text-gray-400 focus:outline-none sm:text-sm/6"
            placeholder="1234-56-78"
            autocomplete="off"
            @focus="!isMusicStart && playMusic()"
          />
          <button
            style="
              color: #fff;
              font-size: 12px;
              font-weight: 500;
              padding: 5px 8px;
              background-color: dimgray;
              border-radius: 6px;
              margin-right: 1px;
              cursor: pointer;
            "
            @click="checkPassword"
          >
            확 인
          </button>
        </div>
        <small v-if="isWrong" style="font-size: 10px; position: relative; color: pink; top: 1"
          >자기야.. 이걸 틀려..?
        </small>
      </div>
    </div>
    <div v-else class="w-[100%] h-[100%] flex flex-col justify-center items-center">
      <motion.img
        layout
        class="absolute"
        v-for="(flower, index) in flowers"
        :key="index"
        :src="flower.backgroundImage"
        :style="flower.style"
        :initial="{ opacity: 0 }"
        :animate="
          step >= 8
            ? {
                opacity: [0, 0, 1],
                transition: {
                  duration: flower.duration + 1,
                  times: [0, flower.duration / (flower.duration + 1), 1],
                },
              }
            : {}
        "
      />
      <div class="absolute">
        <motion.div class="date-box text-[#fff]">
          <span v-show="step >= 0"> <RowValue :value="roundedYear" /> </span>
          <span
            v-show="step >= 1"
            :initial="{ opacity: 0 }"
            :animate="{ opacity: 1, transition: { duration: 1 } }"
          >
            년 <RowValue v-show="step === 1" :value="roundedMonth" />
          </span>
          <span
            v-show="step >= 2"
            :initial="{ opacity: 0 }"
            :animate="{ opacity: 1, transition: { duration: 1 } }"
          >
            월 <RowValue :value="roundedDay" />
          </span>
          <span
            v-show="step >= 3"
            :initial="{ opacity: 0 }"
            :animate="{ opacity: 1, transition: { duration: 1 } }"
          >
            일
          </span>
        </motion.div>

        <motion.div class="date-box text-[#fff]">
          <motion.span
            layout
            v-show="step >= 7"
            :initial="{ opacity: 0 }"
            :animate="step >= 7 ? { opacity: 1, transition: { duration: 2 } } : {}"
            >천사가 하늘에서 내려온 지&nbsp;</motion.span
          >
          <motion.span v-show="step >= 4" layout>
            <RowValue :value="roundedAllDay" />
          </motion.span>
          <motion.span
            layout
            v-show="step >= 5"
            :initial="{ opacity: 0 }"
            :animate="step >= 5 ? { opacity: 1, transition: { duration: 2 } } : {}"
            >&nbsp;일&nbsp;</motion.span
          >
          <motion.span
            layout
            v-show="step >= 6"
            :initial="{ opacity: 0 }"
            :animate="step >= 6 ? { opacity: 1, transition: { duration: 2 } } : {}"
            >되는 날</motion.span
          >
        </motion.div>

        <!-- <motion.div
          layout
          class="m-8 rounded-full text-[#fff] flex justify-center align-center"
          style="margin-top: 5vh"
          :animate="
            step >= 9 ? { width: '100vw', height: '20vh', transition: { duration: 2 } } : {}
          "
        >
          <div
            class="w-[30vw] h-[30vw]"
            :style="{
              backgroundImage: `url('../assets/images/flower.png')`,
              backgroundRepeat: 'no-repeat',
              backgroundSize: 'contain',
              transform: `translateY(2vh)`,
            }"
          ></div>
        </motion.div> -->
        <div class="flex justify-center align-center w-[100vw]">
          <motion.div
            layout
            class="m-8 rounded-full bg-[#f14d4f] flex justify-center align-center"
            style="margin-top: 10vh; overflow: hidden"
            :animate="
              step >= 12
                ? { width: 0, height: 0, transition: { duration: 1.5 } }
                : step >= 9
                  ? { width: '30vw', height: '30vw', transition: { duration: 3 } }
                  : {}
            "
          >
            <div
              id="rounded-img"
              class="w-[60%] h-[120%] flex justify-center align-center"
              :style="{
                backgroundImage: `url('@/assets/images/2.png')`,
                backgroundRepeat: 'no-repeat',
                backgroundSize: 'contain',
                transform: `translateY(2vh)`,
              }"
            >
              <motion.img
                v-show="step == 11"
                src="@/assets/images/heart.png"
                class="absolute w-[5vw] h-[5vw] cursor-pointer"
                :initial="{ scale: 0, opacity: 0 }"
                :animate="{ scale: [1, 1.1], opacity: 1, x: '1.5vw', y: '15vw' }"
                :transition="{
                  duration: 0,
                  scale: {
                    duration: 0.6,
                    repeat: Infinity,
                  },
                }"
                @click="step = 12"
              />
            </div>
          </motion.div>

          <template v-if="step >= 10">
            <motion.div
              class="absolute text-[pink] text-shadow-w"
              :animate="{ x: '-10vw', y: '5.7vw', rotate: '-38deg', transition: { duration: 1 } }"
              >생</motion.div
            >
            <motion.div
              class="absolute text-[coral] text-shadow-w"
              :animate="{ x: '-5.5vw', y: '3.1vw', rotate: '-20deg', transition: { duration: 1 } }"
              >일</motion.div
            >
            <motion.div
              class="absolute text-[lime] text-shadow-w"
              :animate="{ y: '2.2vw', rotate: '0deg', transition: { duration: 1 } }"
              >축</motion.div
            >
            <motion.div
              class="absolute text-[gold] text-shadow-w"
              :animate="{ x: '5.5vw', y: '3.1vw', rotate: '20deg', transition: { duration: 1 } }"
              >하</motion.div
            >
            <motion.div
              class="absolute text-[yellow] text-shadow-w"
              style="color: purple"
              :animate="{ x: '10vw', y: '5.7vw', rotate: '40deg', transition: { duration: 1 } }"
              >해</motion.div
            ></template
          >
        </div>
      </div>
      <div
        class="absolute top-0 bottom-0 left-0 right-0 pt-[15vw]"
        style="display: flex; justify-content: center; align-items: center"
        v-if="step >= 13"
      >
        <motion.div
          class="bg-[#f14d4f] rounded-full"
          :initial="{ opacity: 0, width: 0, height: 0 }"
          :animate="{
            opacity: 1,
            width: wWidth,
            height: wHeight,
            transition: { duration: 1.5, ease: 'easeInOut', opacity: { duration: 0 } },
          }"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted, ref, watch } from 'vue'
import { motion, RowValue, useMotionValue, animate, useTransform } from 'motion-v'

const password = ref('')
const isWrong = ref(false)

const checkPassword = () => {
  if (password.value == '2019-05-28') {
    isStart.value = true
  } else {
    isWrong.value = true
  }
}

// ✅ 오디오 객체 만들기
const myAudio = new Audio('@/assets/audio/love_is_at_the_Milkyway_Cafe.mp3')
myAudio.volume = 1 // 볼륨 조절 가능 (0 ~ 1)
myAudio.loop = true // 볼륨 조절 가능 (0 ~ 1)
const isMusicStart = ref(false)

const playMusic = () => {
  myAudio.play()
  console.log('start')
  isMusicStart.value = true
}

const wWidth = ref('300vw')
const wHeight = ref('300vw')

const sakuras = new Array(80).fill().map(() => {
  const size = Math.random() * 1 + 1
  return {
    backgroundImage: `/assets/images/s${parseInt(Math.random() * 4 + 1)}.png`,
    duration: Math.random() * 40,
    style: {
      width: size + 'vw',
      height: size + 'vw',
      left: Math.random() * 100 + 'vw',
    },
    rand: parseInt(Math.random() * 10) % 2 === 0,
  }
})

const flowers = new Array(150).fill().map(() => {
  const size = Math.random() * 3 + 1
  return {
    backgroundImage: `/assets/images/f${parseInt(Math.random() * 7 + 1)}.png`,
    duration: Math.random() * 5,
    style: {
      width: size + 'vw',
      height: size + 'vw',
      top: Math.random() * 100 + 'vh',
      left: Math.random() * 100 + 'vw',
    },
  }
})

const year = useMotionValue(0)
const month = useMotionValue(0)
const day = useMotionValue(0)
const allDay = useMotionValue(0)

const roundedYear = useTransform(() => Math.round(year.get()))
const roundedMonth = useTransform(() => Math.round(month.get()))
const roundedDay = useTransform(() => Math.round(day.get()))
const roundedAllDay = useTransform(() => Math.round(allDay.get()))

const isStart = ref(false)

const step = ref(0)

watch(step, async () => {
  if (step.value === 12) {
    await new Promise((resolve) => setTimeout(resolve, 2000))
    step.value = 13
    await new Promise((resolve) => setTimeout(resolve, 5000))
    step.value = 14
  }
})

watch(isStart, async () => {
  if (isStart.value) {
    await new Promise((resolve) => setTimeout(resolve, 1000))
    await animate(year, [0, 1400, 1950, 2000, 2025], {
      duration: 5,
      times: [0, 0.2, 0.4, 0.6, 1],
      ease: 'linear',
    })
    step.value = 1
    await animate(month, 5, { duration: 2, ease: 'linear' })
    step.value = 2
    await animate(day, 19, { duration: 5, ease: 'linear' })
    step.value = 3
    await new Promise((resolve) => setTimeout(resolve, 1000))
    step.value = 4
    await animate(allDay, [0, 6500, 9400, 9850, 9862], {
      duration: 7,
      times: [0, 0.2, 0.4, 0.6, 1],
      ease: 'linear',
    })
    await new Promise((resolve) => setTimeout(resolve, 1000))
    step.value = 5
    await new Promise((resolve) => setTimeout(resolve, 2000))
    step.value = 6
    await new Promise((resolve) => setTimeout(resolve, 2000))
    step.value = 7
    await new Promise((resolve) => setTimeout(resolve, 3000))
    step.value = 8
    await new Promise((resolve) => setTimeout(resolve, 4500))
    step.value = 9
    await new Promise((resolve) => setTimeout(resolve, 3000))
    step.value = 10
    await new Promise((resolve) => setTimeout(resolve, 2000))
    step.value = 11
  }
})

const resizeHandler = () => {
  wWidth.value = window.innerWidth > window.innerHeight ? '300vw' : '300vh'
  wHeight.value = window.innerWidth > window.innerHeight ? '300vw' : '300vh'
}

onMounted(() => {
  window.addEventListener('resize', resizeHandler)
})

onUnmounted(() => {
  window.removeEventListener('resize', resizeHandler)
  myAudio.pause()
})
</script>

<style scoped>
.date-box {
  display: flex;
  justify-content: center;
  align-items: center;
}

.text-shadow-w {
  text-shadow:
    rgb(255, 255, 255) 3px 0px 1px,
    rgb(255, 255, 255) -3px 0px 1px,
    rgb(255, 255, 255) 0px 3px 1px,
    rgb(255, 255, 255) 0px -3px 1px,
    rgb(255, 255, 255) 3px 3px 1px,
    rgb(255, 255, 255) -3px -3px 1px,
    rgb(255, 255, 255) 3px -3px 1px,
    rgb(255, 255, 255) -3px 3px 1px,
    rgb(255, 255, 255) 0px 0px 5px;
}
</style>
