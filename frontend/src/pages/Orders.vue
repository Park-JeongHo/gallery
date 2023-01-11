<template>
  <div class="orders">
    <div class="container">
      <table class="table table-borderd">
        <thead>
        <tr>
          <td>번호</td>
          <td>주문자명</td>
          <td>주소</td>
          <td>결제수단</td>
          <td>구입 항목</td>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(o,idx) in state.orders" :key="idx">
          <td>{{state.orders.length - idx}}</td>
          <td>{{o.name}}</td>
          <td>{{o.address}}</td>
          <td>{{o.payment}}</td>
          <td>
            <div v-for="(i,idx1) in o.items" :key="idx1">
                {{i.name}}
            </div>
          </td>

        </tr>
        </tbody>
      </table>

    </div>
  </div>
</template>

<script>
import { reactive} from "vue";
import axios from "axios";
import lib from "@/scripts/lib";

export default {
  setup() {
    const state = reactive({
     orders : []
    })
    axios.get("/api/orders").then(({data})=>{
      state.orders = [];
      for(let d of data){
        if (d.items) {
          d.items = JSON.parse(d.items);
        }
        state.orders.push(d);
      }

    })
    return {state, lib}
  }
}
</script>

<style scoped>
.table{
  margin-top: 30px;
}
.table > tbody{
  border-top: 1px solid #eee;
}
</style>