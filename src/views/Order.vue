<script setup lang="ts">
import { ref } from 'vue'

const date = ref()
const orders = ref([
  {
    id: "12345678",
    phoneNumber: "010-1234-5678",
    amount: "5,000",
    orderedDate: "1970-01-01 00:00:00",
    expiration: "1970-01-06 23:59:59",
    isExchanged: false,
    showDetails: false
  },
  {
    id: "12345678",
    phoneNumber: "010-1234-5678",
    amount: "5,000",
    orderedDate: "1970-01-01 00:00:00",
    expiration: "1970-01-06 23:59:59",
    isExchanged: false,
    showDetails: false
  },
  {
    id: "12345678",
    phoneNumber: "010-1234-5678",
    amount: "5,000",
    orderedDate: "1970-01-01 00:00:00",
    expiration: "1970-01-06 23:59:59",
    isExchanged: false,
    showDetails: false
  }
])

function showDetails(index: number) {
  orders.value[index].showDetails = !orders.value[index].showDetails
}
</script>

<template>
  <div class="container">
    <main class="main">
      <h1 class="typography">주문</h1>
      <label class="typography">모든 주문 내역을 날짜 범위, 전화번호를 조건으로 설정하여 조회할 수 있습니다.</label>
      <div style="height: 32px"></div>
      <div class="condition-container">
        <div class="condition">
          <label class="typography label">날짜 범위</label>
          <VueDatePicker v-model="date" range auto-apply locale="ko" format="yyyy/MM/dd" :partial-range="false"
            :enable-time-picker="false"></VueDatePicker>
        </div>
        <div class="condition">
          <label class="typography label">전화번호</label>
          <div class="input">
            <input />
          </div>
        </div>
      </div>
      <div style="margin-top: 10px; margin-bottom: 30px">
        <button class="primary-button">조회</button>
      </div>
      <div class="table-container">
        <div class="scroll-container">
          <table class="table">
            <thead>
              <tr>
                <th style="width: 50px"></th>
                <th>주문번호</th>
                <th>전화번호</th>
                <th>금액</th>
                <th>주문 일시</th>
                <th>만료 일시</th>
                <th>교환 여부</th>
              </tr>
            </thead>
            <tbody v-for="(order, index) in orders">
              <tr>
                <td><span class="material-symbols-outlined expand" :class="{ expanded: order.showDetails }"
                    @click="showDetails(index)">chevron_right</span></td>
                <td>{{ order.id }}</td>
                <td>{{ order.phoneNumber }}</td>
                <td>{{ order.amount }}</td>
                <td>{{ order.orderedDate }}</td>
                <td>{{ order.expiration }}</td>
                <td>{{ order.isExchanged ? "O" : "X" }}</td>
              </tr>
              <tr v-if="order.showDetails">
                <td class="detail" colspan="7">detail</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.condition-container {
  display: flex;
}

.condition {
  width: 100%;
  margin-right: 20px;
}

.condition:last-child {
  margin-right: 0px;
}

.label {
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

.detail {
  overflow: hidden;
  background-color: rgba(242, 244, 246, 0.5);
  text-align: left;
}

.expand {
  cursor: pointer;
  transition: transform .15s ease-out;
}

.expanded {
  transform: rotate(90deg);
}

th {
  padding: 8px 12px;
  font-weight: normal;
  border-left: none;
  background: rgb(242, 244, 246);
}

td {
  padding: 8px 12px;
  font-weight: normal;
  border-left: none;
}

tr>td {
  text-align: center;
}
</style>
