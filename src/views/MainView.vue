<template>
  <div class="w-full h-[300vh] bg-[#f14d4f]" ref="containerRef">
    <!-- 친구 -->
    <template v-if="isOpen">
      <motion.div
        v-for="(_style, _index) in friends"
        :key="_index"
        class="fixed w-[30vw] h-[50vh] text-white flex justify-center items-center aspect-1/2"
        style="background-repeat: no-repeat; background-size: contain"
        :style="_style"
      />
    </template>
    <motion.div
      v-if="isScrollEnd"
      @click="handleOpen"
      class="fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 h-[calc(15vw+15vh)] aspect-square"
      :style="{
        cursor: 'pointer',
        backgroundImage: `url(${isOpen ? './assets/images/cake.png' : './assets/images/box.png'})`,
        backgroundRepeat: 'no-repeat',
        backgroundSize: 'contain',
      }"
      :initial="{ opacity: 0 }"
      :animate="{
        opacity: scrollDepth ? 1 : 0,
        rotate: isOpen ? 0 : [0, -5, 0, 5, 0, -5, 0, 5, 0],
      }"
      :transition="{
        rotate: { duration: 2, repeat: isOpen ? null : Infinity, ease: 'linear' },
      }"
    />
    <!-- 연지 -->
    <motion.div
      class="fixed w-[30vw] h-[50vh] text-white flex justify-center items-center aspect-1/2"
      style="
        background-image: url('./assets/images/2.png');
        background-repeat: no-repeat;
        background-size: contain;
      "
      :style="{
        right: _xPos,
        top: rightTop,
        rotate: rotateValue,
        width: width,
        height: height,
      }"
    />
    <!-- 현광 -->
    <motion.div
      class="fixed w-[30vw] h-[50vh] text-white flex justify-center items-center aspect-1/2"
      style="
        background-image: url('./assets/images/1.png');
        background-repeat: no-repeat;
        background-size: contain;
      "
      :style="{
        left: xPos,
        top: leftTop,
        rotate: rotateValue,
        width: width,
        height: height,
      }"
    />

    <motion.div
      v-if="isOpen"
      class="fixed top-[0] bottom-[0] left-[0] right-[0] flex justify-center items-center bg-[#d9d9d9]"
      :initial="{ opacity: 1 }"
      :animate="{ opacity: [1, 1, 0.9, 0.6, 0.3, 0] }"
      :transition="{ duration: 5, times: [0, 0.5, 0.6, 0.7, 0.9, 1] }"
    >
      <motion.div
        v-if="isOpen"
        class="w-[80vw] h-[80vh]"
        style="
          background-image: url('./assets/images/boom.png');
          background-repeat: no-repeat;
          background-size: cover;
        "
      />
    </motion.div>

    <div class="fixed top-[8vh] left-[37.5vw]" v-if="cameraOpen">
      <video
        v-if="!didTakePhoto"
        ref="videoElement"
        id="video"
        autoplay
        playsinline
        class="border"
        style="width: 25vw; height: 25vh"
      ></video>
      <canvas
        v-else
        ref="canvasElement"
        id="canvas"
        class="mt-4 border"
        style="width: 25vw; height: 25vh"
      ></canvas>
      <button @click="didTakePhoto ? retryTakePhoto : takePhoto">
        📸 {{ didTakePhoto ? '다시찍기' : '촬영하기' }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { motion, useMotionValueEvent, useScroll, useTransform } from 'motion-v'
import { watch } from 'vue'
import { nextTick, onBeforeUnmount, onMounted, ref } from 'vue'

const emit = defineEmits(['stop-audio'])
const wWidth = window.innerWidth
const wHeight = window.innerHeight

const friends = [
  {
    backgroundImage: "url('./assets/images/p1.png')",
    right: '0vw',
    top: '0vh',
    rotate: '18deg',
    width: wWidth > wHeight ? '30vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '35vh',
    aspectRatio: 1,
  },
  {
    backgroundImage: "url('./assets/images/p2.png')",
    left: '40vw',
    top: '0vh',
    rotate: '-20deg',
    width: wWidth > wHeight ? '18vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '30vh',
    aspectRatio: 1,
  },
  {
    backgroundImage: "url('./assets/images/p3.png')",
    left: '-5vw',
    top: '20vh',
    rotate: '20deg',
    width: wWidth > wHeight ? '20vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '50vh',
    aspectRatio: 1 / 2,
  },
  {
    backgroundImage: "url('./assets/images/p4.png')",
    right: '5vw',
    bottom: '-5vh',
    rotate: '0deg',
    width: wWidth > wHeight ? '18vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '30vh',
    aspectRatio: 1,
  },
  {
    backgroundImage: "url('./assets/images/p5.png')",
    left: '0',
    top: '70vh',
    rotate: '0deg',
    width: wWidth > wHeight ? '18vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '30vh',
    aspectRatio: 1,
  },
  {
    backgroundImage: "url('./assets/images/p6.png')",
    left: '86vw',
    top: '65vh',
    rotate: '0deg',
    width: wWidth > wHeight ? '20vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '50vh',
    aspectRatio: 1 / 2,
  },
  {
    backgroundImage: "url('./assets/images/p7.png')",
    left: '20vw',
    bottom: '-15vh',
    rotate: '0deg',
    width: wWidth > wHeight ? '25vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '60vh',
    aspectRatio: 1 / 1.5,
  },
  {
    backgroundImage: "url('./assets/images/p8.png')",
    left: '55vw',
    bottom: '10vh',
    rotate: '0deg',
    width: wWidth > wHeight ? '18vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '30vh',
    aspectRatio: 1,
  },
  {
    backgroundImage: "url('./assets/images/p9.png')",
    right: '-5vw',
    top: '40vh',
    rotate: '0deg',
    width: wWidth > wHeight ? '18vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '22vh',
    aspectRatio: 1,
  },
  {
    backgroundImage: "url('./assets/images/p10.png')",
    left: '-3vw',
    top: '-4.8vh',
    rotate: '180deg',
    width: wWidth > wHeight ? '18vw' : 'auto',
    height: wWidth > wHeight ? 'auto' : '25vh',
    aspectRatio: 1,
  },
]

const containerRef = ref(null)

const { scrollYProgress } = useScroll({
  target: containerRef,
  offset: ['start start', 'end end'],
})

// ✅ 1. 이동 값 (0 ~ 0.5구간만 이동)
const xPos = useTransform(scrollYProgress, [0, 0.4, 1], ['-50vw', '30vw', '20vw'])
const _xPos = useTransform(scrollYProgress, [0, 0.4, 1], ['-50vw', '30vw', '20vw'])

// ✅ 2. 회전 (걷는 느낌)
const rotateValue = useTransform(
  scrollYProgress,
  [0, 0.1, 0.2, 0.3, 0.4],
  ['-10deg', '10deg', '-10deg', '10deg', '0deg'], // 0.5지점에선 정지
)

// ✅ 3. 스케일 (0.5~1 구간에서 축소)
const width = useTransform(
  scrollYProgress,
  [0.5, 0.9],
  [wWidth > wHeight ? '20vw' : 'auto', wWidth > wHeight ? '15vw' : 'auto'],
)
const height = useTransform(
  scrollYProgress,
  [0.5, 0.9],
  [wWidth > wHeight ? 'auto' : '50vh', wWidth > wHeight ? 'auto' : '30vh'],
)

// ✅ 4. y 위치 (0.5~1 구간에서 위로 이동)
const leftTop = useTransform(scrollYProgress, [0.5, 0.9], ['30vh', '10vh'])
const rightTop = useTransform(scrollYProgress, [0.5, 0.9], ['30vh', '10vh'])

// ✅ 디버깅용
const scrollDepth = useTransform(scrollYProgress, [0, 0.9, 1], [0, 0, 1])

// ✅ 오디오 객체 만들기
const birthdaySong = new Audio('./assets/audio/happy-birthday.mp3')
birthdaySong.volume = 1 // 볼륨 조절 가능 (0 ~ 1)
birthdaySong.loop = true // 볼륨 조절 가능 (0 ~ 1)

const isScrollEnd = ref(false)

const isOpen = ref(false)

useMotionValueEvent(scrollYProgress, 'change', (latest) => {
  if (latest >= 0.99) {
    isScrollEnd.value = true
  }
})

const scrollNext = () => {
  // 전체 스크롤 길이 계산
  const scrollHeight = document.body.scrollHeight - window.innerHeight

  let current = 0
  const speed = scrollHeight / 600 // ✅ 속도 (값을 키우면 빠름)

  const interval = setInterval(() => {
    current += speed
    window.scrollTo(0, current)

    // 끝에 도달하면 멈춤
    if (current >= scrollHeight) {
      clearInterval(interval)
    }
  }, 10) // ✅ 16ms마다 실행 (60fps)
}

const cameraOpen = ref(false)

const handleOpen = () => {
  if (!isOpen.value) {
    isOpen.value = true

    emit('stop-audio')

    setTimeout(() => {
      birthdaySong.play()
    }, 1000)

    setTimeout(() => {
      cameraOpen.value = true
    }, 10000)
  }
}

const disableScroll = (e) => {
  e.preventDefault()
}

const didTakePhoto = ref(false)
const canvasElement = ref(null)
const videoElement = ref(null)
const context = ref(null)

const takePhoto = async () => {
  const image = videoElement.value

  didTakePhoto.value = true

  await nextTick(async () => {
    context.value = await canvasElement.value.getContext('2d')
    context.value.drawImage(image, 0, 0, canvasElement.value.width, canvasElement.value.height)
  })
}

const retryTakePhoto = async () => {
  didTakePhoto.value = false

  await nextTick(() => {
    setVideo()
  })
}

const setVideo = () => {
  // 카메라 접근
  navigator.mediaDevices
    .getUserMedia({ video: true })
    .then((stream) => {
      videoElement.value.srcObject = stream
    })
    .catch((err) => {
      console.error('카메라 접근 실패:', err)
      alert('카메라 접근 실패.')
      cameraOpen.value = false
    })
}

watch(cameraOpen, () => {
  if (cameraOpen.value) {
    setVideo()
  }
})

onMounted(() => {
  window.addEventListener('wheel', disableScroll, { passive: false })
  window.addEventListener('touchmove', disableScroll, { passive: false })
  scrollNext()
})

onBeforeUnmount(() => {
  window.removeEventListener('wheel', disableScroll)
  window.removeEventListener('touchmove', disableScroll)
})
</script>

<style lang="scss" scoped></style>
