<template>
  <div>
    <img class="room-img" :src="room.image" />
    <!-- props는 수정할 수 없음 -->
    <!-- custom event를 통해 부모 컴포넌트에게 메시지 줄 수 있음 -->
    <h4 @click="sendOpenModal()" class="room-title">
      {{ room.title }}
    </h4>
    <p>{{ room.price }}원</p>
  </div>
</template>

<script>
export default {
  name: "CardComponent",
  props: {
    room: Object,
  },
  methods: {
    sendOpenModal() {
      this.$emit("openModal", this.room.id);
    },
  },
};
</script>

<style></style>
