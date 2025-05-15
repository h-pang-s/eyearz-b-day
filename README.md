# eyearz-b-day

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

# 구성

- 초기 시작 화면

  - 1안 (하트비트)일 수 올라감 -> 우리가 만난 기간 & 문구 나옴 6주년
  - 2안 6주년 -> 우리가 만난 기간 -> 일수로 변환(일수 카운트되며 올라가짐)

  - 비밀번호 입력 해야 다음 진행됨 -> 비밀번호는 우리 만난 날

- 비밀번호 입력 후

  - (입력후 초기 화면)

    - 어린 시절 이미지 좌, 우 에서 화면 가운데로 천천히 다가옴 -> (볼 뽀뽀) // 볼뽀뽀가 되면 스크롤 되게 변경
    - 스크롤 하면 이미지가 header logo 로 사이즈 천천히 줄어들면서 변경 & GIFT, LETTER, CAMERA, Gallery, HBD nav 가 나옴

  - GIFT

  - LETTER

    - 간단하게, 폰트 귀여운거 해서 편지

  - CAMERA

    - 웹캠 전용으로 캡처해서 쓰던가 하게.. (폴라로이드 형식)

  - 갤러리

    - 사진 캐러셀 ( 추억 사진 캐러셀 )

  - HBD
    - 선물상자 클릭 -> 어린시절 사진 + 케이크 사진 & 생일 축하 문구
    - 일정 시간 후 케이크에 불 붙는 이미지 생긴 후 생일축하 노래 흘러나옴
