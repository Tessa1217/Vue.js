<!-- App.vue => index.html로 HTML 컴파일 -->
<template>
  <!-- v-if -->
  <!-- 이벤트 핸들러 v-on:event 또는 @event -->
  <!-- props: 부모 컴포넌트(App)에서 자식 컴포넌트(Modal)로 데이터 전달 -->
  <modal-component
    :roomList="roomList"
    :roomIdx="roomIdx"
    :modalOpenState="modalOpenState"
    @closeModal="closeModal()"
  />
  <!-- 반복문: v-for -->
  <!-- :key => 반복문으로 생성한 요소들 각각 구분하기 위한 속성 -->
  <div class="menu">
    <ul>
      <li v-for="item in menuList" :key="item">
        <a href="#">{{ item }}</a>
      </li>
    </ul>
  </div>
  <!-- 축약해둔 컴포넌트 -->
  <discount-component />
  <!-- {{ data }} 테이터 바인딩: Vue의 실시간 자동 렌더링 사용 -->
  <!-- 속성에서 데이터바인딩할 때에는 :data -->
  <card-component
    @openModal="
      openModal();
      roomIdx = $event;
    "
    v-for="(room, i) in roomList"
    :key="i"
    :room="room"
  />
</template>

<script>
import onerooms from "./data/oneroom.js";
import DiscountComponent from "./Discount.vue";
import ModalComponent from "./Modal.vue";
import CardComponent from "./Card.vue";

export default {
  name: "App",
  // 데이터 속성: 일종의 데이터 보관함
  data() {
    return {
      roomList: onerooms,
      menuList: ["Home", "Products", "About"],
      // 동적인 UI 상태 데이터 저장
      modalOpenState: false,
      roomIdx: "",
    };
  },
  methods: {
    openModal() {
      this.modalOpenState = true;
    },
    closeModal() {
      this.modalOpenState = false;
    },
  },
  components: {
    DiscountComponent: DiscountComponent,
    ModalComponent: ModalComponent,
    CardComponent: CardComponent,
  },
};
</script>

<style>
body {
  margin: 0;
}

div {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.menu {
  background: darkslateblue;
  padding: 10px;
}

.menu ul {
  padding-left: 0px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.menu ul li {
  display: inline-block;
}

.menu ul li a {
  color: white;
  font-weight: bold;
  text-decoration: none;
}

.report-btn {
  background: red;
  border: none;
  padding: 3px 5px;
  color: white;
  font-weight: bold;
}

.room-img {
  width: 100%;
  margin-top: 40px;
}

.room-title:hover {
  cursor: pointer;
  text-decoration: underline;
  font-weight: bold;
}

.black-bg {
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  padding: 20px;
}

.white-bg {
  width: 90%;
  background: white;
  border-radius: 8px;
  padding: 20px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.close-btn {
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  color: white;
  font-weight: bold;
  background-color: gray;
}

button:hover {
  cursor: pointer;
}
</style>
