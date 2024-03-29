<script setup lang="ts">
import { useRouter } from 'vue-router'
import { Ref, ref } from 'vue'
import { AxiosError } from 'axios'

import { useStore } from '@/store'
import logo from '@/assets/h4pay_logo.png'
import Modal from '@/components/Modal.vue'
import { postData } from '@/api'
import PhoneNumberConverter from '@/util/PhoneNumberConverter'

const router = useRouter()
const store = useStore()
const isModalOpened = ref(false)
const phoneNumber: Ref<string> = ref("")
const password: Ref<string> = ref("")

const isChrome = window.navigator.userAgent.toLowerCase().indexOf('chrome')

if (isChrome == -1) {
  alert('본 웹사이트는 Google Chrome에 최적화되어 있습니다.')
}

async function submitForm() {
  const signIn: SignIn = {
    phoneNumber: PhoneNumberConverter.removeNonNumerics(phoneNumber.value),
    password: password.value
  }

  await postData<TokenInfo>('/login', signIn).then(res => {
    store.setAccessToken(res.data.accessToken)
    router.push('/dashboard')
  }).catch((e: AxiosError) => {
    if (e.response?.status == 400) {
      alert('전화번호 또는 비밀번호가 바르지 않습니다.')
    } else if (e.response?.status == 500) {
      alert('서버 내부 오류가 발생했습니다.')
    } else {
      alert('알 수 없는 오류가 발생했습니다.')
    }
  })
}
</script>

<template>
  <div>
    <main class="main">
      <div class="content">
        <div class="header">
          <div class="logo-container">
            <img class="logo" :src="logo" />
            <div class="logo-typography">
              <div style="
                font-weight: bold;
                font-size: 3em;
                line-height: 1;
                margin-bottom: 5px;
              ">
                H4Pay
              </div>
              <div style="font-size: 2em; line-height: 1">매니저</div>
            </div>
          </div>
          <div>
            <h1 class="typography-h5" style="margin-top: 1em;">공지사항</h1>
            <hr />
            <div style="height: 20vh"></div>
          </div>
        </div>
        <div class="card">
          <div class="signin-header">
            <h1 class="typography-h5">로그인</h1>
          </div>
          <form @submit.prevent="submitForm">
            <div>
              <label class="input-label">전화번호</label>
              <div class="input">
                <input v-model="phoneNumber" @input="() => { phoneNumber = PhoneNumberConverter.addDashes(phoneNumber) }" />
              </div>
            </div>

            <div class="m-25">
              <label class="input-label">비밀번호</label>
              <div class="input">
                <input type="password" v-model="password" />
              </div>
            </div>

            <div class="button-container">
              <button class="primary-button" type="button" @click="isModalOpened = true">
                회원가입
              </button>
              <button class="primary-button" style="margin-left: 10px" type="submit">로그인</button>
            </div>
          </form>
        </div>
        <Modal v-if="isModalOpened" @close-modal="isModalOpened = false">
          <div class="signup">
            <h1 class="typography-h5">회원가입</h1>
            <div class="signup-content">
              <div class="signin-input">
                <label class="input-label">전화번호</label>
                <div class="input">
                  <input />
                </div>
                <label class="input-label">전화번호</label>
                <div class="input">
                  <input />
                </div>
                <label class="input-label">전화번호</label>
                <div class="input">
                  <input />
                </div>
                <label class="input-label">전화번호</label>
                <div class="input">
                  <input />
                </div>
                <label class="input-label">전화번호</label>
                <div class="input">
                  <input />
                </div>
                <label class="input-label">전화번호</label>
                <div class="input">
                  <input />
                </div>
              </div>
            </div>
          </div>
        </Modal>
      </div>
      <footer>
        본 웹사이트는 Google Chrome 및 1920x1080 이상의 해상도에 최적화되어 있습니다.
      </footer>
    </main>
  </div>
</template>

<style scoped>
hr {
  border: 1px solid lightgrey;
}

.main {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.content {
  display: flex;
}

.logo-container {
  display: flex;
  align-items: center;
  flex-grow: 1;
}

.logo {
  width: 100px;
  border-radius: var(--logo-border-radius);
}

.logo-typography {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-left: 20px;
}

.signin-header {
  margin-bottom: 40px;
}

.signup {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.signup-content {
  width: 80%;
}

.m-25 {
  margin-top: 25px;
}

.card {
  width: 500px;
  background-color: white;
  padding: 1rem 1.2rem;
  border-radius: 12px;
  box-shadow: 0px 2px 20px #d9d9d9;
  border: 1px solid #d9d9d9;
}

.header {
  display: flex;
  flex-direction: column;
  width: 500px;
  margin-right: 50px;
}

.input-label {
  font-weight: semibold;
  display: inline-block;
  margin: 0.5rem 0;
}

.input {
  display: flex;
  height: 48px;
  line-height: 20px;
  border: none;
  outline: none;
  box-shadow: inset 0 0 0 1px rgba(0, 27, 55, 0.1);
  transition: all 0.2s ease;
  border-radius: 8px;
}

.input:hover {
  box-shadow: inset 0 0 0 2px #83bdff;
}

.input:focus-within {
  box-shadow: inset 0 0 0 2px slateblue;
}

.input input {
  width: 100%;
  background: none;
  border: none;
  outline: none;
  padding: 0 18px;
  font-size: 18px;
}
</style>
