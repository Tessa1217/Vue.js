<template>
  <div class="black-bg" v-if="modalOpenState == true">
    <div class="white-bg">
      <h4>{{ roomList[roomIdx].title }}</h4>
      <img class="room-img" :src="roomList[roomIdx].image" />
      <p>월세: {{ roomList[roomIdx].price }}원</p>
      <p>{{ roomList[roomIdx].content }}</p>
      <!-- 사용자 입력한 값 데이터에 저장 == v-model="데이터 이름" -->
      <!-- <input type="number" v-model="month" /> -->
      <select v-model="month">
        <option v-for="i in 11" :key="i">{{ i }}</option>
      </select>
      <p>{{ month }}개월 선택. 총금액: {{ roomList[roomIdx].price * month }}</p>
      <button class="close-btn" @click="sendCloseModal()">닫기</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalComponent",
  data() {
    return {
      totalPrice: "",
      month: 1,
    };
  },
  props: {
    roomList: Array,
    roomIdx: Number,
    modalOpenState: Boolean,
  },
  methods: {
    sendCloseModal() {
      this.$emit("closeModal");
    },
  },
};
</script>

<style></style>
