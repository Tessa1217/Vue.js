<!-- App.vue => index.html로 HTML 컴파일 -->
<template>
  <div>
    <!-- v-if -->
    <!-- 이벤트 핸들러 v-on:event 또는 @event -->
    <!-- props: 부모 컴포넌트(App)에서 자식 컴포넌트(Modal)로 데이터 전달 -->
    <Transition name="fade">
      <modal-component
        :roomList="roomList"
        :roomIdx="roomIdx"
        :modalOpenState="modalOpenState"
        @closeModal="closeModal()"
      />
    </Transition>
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
    <discount-component
      v-if="discountState == true"
      :discountRate="discountRate"
    />
    <div class="button-container">
      <button @click="sortByLowPrice()">낮은 가격순</button>
      <button @click="sortByHighPrice()">높은 가격순</button>
      <button @click="sortByName()">가나다순</button>
      <button @click="belowFifty()">50만원 이하만</button>
      <button @click="sortBack()">원래대로 정렬</button>
    </div>
    <!-- {{ data }} 테이터 바인딩: Vue의 실시간 자동 렌더링 사용 -->
    <!-- 속성에서 데이터바인딩할 때에는 :data -->
    <card-component
      v-for="(room, i) in roomList"
      :key="i"
      :room="room"
      @openModal="
        openModal();
        roomIdx = $event;
      "
    ></card-component>
  </div>
</template>

<script>
import onerooms from "./data/oneroom.js";
import DiscountComponent from "./Discount.vue";
import ModalComponent from "./Modal.vue";
import CardComponent from "./Card.vue";

let discountInterval;

export default {
  name: "App",
  // html에 장착되니 상태
  // mounted() {
  //   // this 사용 시 arrow function 사용
  //   setTimeout(() => {
  //     this.discountState = false;
  //   }, 2000);
  // },
  mounted() {
    let discountMinus = () => {
      this.discountRate--;
    };
    discountInterval = setInterval(discountMinus, 1000);
  },
  beforeUpdate() {
    if (this.discountRate == 0) {
      clearInterval(discountInterval);
    }
  },
  // 데이터 속성: 일종의 데이터 보관함
  data() {
    return {
      roomList: onerooms,
      menuList: ["Home", "Products", "About"],
      // 동적인 UI 상태 데이터 저장
      modalOpenState: false,
      roomIdx: "",
      originalRoomList: [...onerooms],
      discountState: true,
      discountRate: 5,
    };
  },
  methods: {
    openModal() {
      this.modalOpenState = true;
    },
    closeModal() {
      this.modalOpenState = false;
    },
    sortByLowPrice() {
      // sort
      this.roomList.sort(function (a, b) {
        return a.price - b.price;
      });
    },
    sortByHighPrice() {
      this.roomList
        .sort(function (a, b) {
          return a.price - b.price;
        })
        .reverse();
    },
    sortByName() {
      this.roomList.sort(function (a, b) {
        return a.title < b.title ? -1 : 1;
      });
    },
    belowFifty() {
      this.roomList = this.roomList.filter((room) => {
        return room.price < 500000;
      });
    },
    sortBack() {
      this.roomList = [...this.originalRoomList];
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

.button-container {
  margin-top: 20px;
}

/* transition 애니메이션, enter-from은 시작, enter-to는 끝, enter-active는 transition */
.fade-enter-from {
  opacity: 0;
  transform: translateY(-50%);
}

.fade-enter-active {
  transition: all 0.8s;
}

.fade-enter-to {
  transform: translateY(0);
  opacity: 1;
}

.fade-leave-from {
  opacity: 1;
}

.fade-leave-active {
  transition: all 0.8s;
}

.fade-leave-to {
  opacity: 0;
}
</style>
