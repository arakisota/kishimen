<template>
  <div class="container">
    <div class="create-container">
      <input
        v-model="meeting_id"
        class="id-input"
        type="text"
        placeholder="Meeting ID"
      />
      <button
        :style="{ backgroundColor: btn_color, color: text_color }"
        class="btn"
        @mouseover="btnOver"
        @mouseleave="btnLeave"
        @click="goMainpage"
      >
        ミーティングに参加
      </button>
      <div class="top-container">
        <span
          :style="{ color: top_page_color }"
          class="top-page"
          @click="goTopPage"
          @mouseover="top_page_color = colors.black"
          @mouseleave="top_page_color = colors.gray"
        >
          Top page
        </span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { JoinMeeting } from '/@/api/index'
import { useUserStore } from '/@/store/index'

const colors = {
  team_color: '#ff971d',
  white: '#ffffff',
  black: '#000000',
  gray: 'gray'
}

let btn_color = ref<string>(colors.white)
let text_color = ref<string>(colors.black)
const meeting_id = ref<string>('')
let top_page_color = ref(colors.gray)

const router = useRouter()
const user_store = useUserStore()

const btnOver = () => {
  btn_color.value = colors.team_color
  text_color.value = colors.white
}
const btnLeave = () => {
  btn_color.value = colors.white
  text_color.value = colors.black
}
const goMainpage = () => {
  if (meeting_id.value === '') {
    alert('Meeting IDが入力されていません')
    return
  }
  JoinMeeting(meeting_id.value)
    .then(res => {
      user_store.set_meeting(meeting_id.value)
    })
    .catch(err => {
      throw err
    })
  router.push('main')
}
const goTopPage = () => {
  router.push('start-page')
}
</script>

<style lang="scss">
input {
  all: unset;
}
.container {
  width: 100vw;
  height: 100vh;
}

.flex-container {
  display: flex;
  justify-content: space-around;
  width: 100%;
  height: 100%;
}

.create-container {
  margin: auto;
  padding: 5% 0 5% 0;
  width: 45%;
  height: 90%;
}
.right-side {
  padding: 5% 0 5% 0;
  width: 45%;
  height: 90%;
}

.discription {
  width: 100%;
  height: 50%;
  background-color: white;
}
.image {
  margin-right: 20px;
  margin-top: 100px;
  width: 100%;
  height: 50%;
}
.id-input {
  margin-top: 15%;
  width: 100%;
  height: 20%;
  font-size: 50px;
  box-shadow: none;
  border-style: solid;
  border-radius: 30px;
  border-color: black;
  transition: 0.5s;
  text-align: center;
}
.btn {
  margin-top: 15%;
  width: 100%;
  height: 20%;
  font-size: 50px;
  box-shadow: none;
  border-style: solid;
  border-radius: 30px;
  border-color: #ff971d;
  transition: 0.5s;
  // background-color: v-bind(btn_color);
  // color: v-bind(text_color);
}
.top-container {
  margin-top: 10px;
}
.top-page {
  margin-top: 5%;
  width: 100%;
  font-size: 30px;
  transition: 0.1s;
  // color: v-bind(top_page_color);
  cursor: default;
}
</style>
