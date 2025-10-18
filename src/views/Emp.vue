<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const name = ref("");
const gender = ref("");
const job = ref("");
const userList = ref([]);
//定义search方法
const search = () => {
  axios
    .get(
      `https://web-server.itheima.net/emps/list?name=${name.value}&gender=${gender.value}&job=${job.value}`
    )
    .then((response) => {
      userList.value = response.data.data;
    });
};
//定义clear方法
const clear = () => {
  name.value = "";
  gender.value = "";
  job.value = "";
  userList.value = [];
  search();
};
// 钩子函数
onMounted(() => {});
</script>

<template>
  <div id="app">
    <div id="center">
      姓名:
      <input type="text" name="name" placeholder="请输入姓名" v-model="name" />

      性别:
      <select name="gender" v-model="gender">
        <option value="1">男</option>
        <option value="2">女</option>
      </select>

      职位:
      <select name="job" v-model="job">
        <option value="1">班主任</option>
        <option value="2">讲师</option>
        <option value="3">学工主管</option>
        <option value="4">教研主管</option>
        <option value="5">咨询师</option>
      </select>

      <button class="btn" type="button" @click="search">查询</button>
      <button class="btn" type="button" @click="clear">清空</button>
    </div>

    <table>
      <tr>
        <th>序号</th>
        <th>姓名</th>
        <th>头像</th>
        <th>性别</th>
        <th>职位</th>
        <th>入职时间</th>
        <th>更新时间</th>
      </tr>

      <!-- v-for 用于列表循环渲染元素 -->
      <tr v-for="(user, index) in userList" :key="user.id">
        <td>{{ index + 1 }}</td>
        <td>{{ user.name }}</td>
        <td><img :src="user.image" /></td>
        <td>
          <span v-if="user.gender == 1">男</span>
          <span v-else-if="user.gender == 2">女</span>
          <span v-else>其他</span>
        </td>
        <td>
          <span v-if="user.job == 1">班主任</span>
          <span v-else-if="user.job == 2">讲师</span>
          <span v-else-if="user.job == 3">学工主管</span>
          <span v-else-if="user.job == 4">教研主管</span>
          <span v-else-if="user.job == 5">咨询师</span>
          <span v-else>其他</span>
        </td>
        <td>{{ user.entrydate }}</td>
        <td>{{ user.updatetime }}</td>
      </tr>
    </table>
  </div>
</template>

<style scoped>
table,
th,
td {
  border: 1px solid #949191;
  border-collapse: collapse;
  line-height: 50px;
  text-align: center;
}

#center,
table {
  width: 60%;
  margin: auto;
}

#center {
  margin-top: 50px;
  margin-bottom: 20px;
}

img {
  width: 50px;
}

input,
select {
  width: 17%;
  padding: 10px;
  margin-right: 30px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.btn {
  background-color: #08dcf3;
  padding: 8px 30px;
  margin-right: 10px;
  border-radius: 8px;
  border: #08dcf3 solid 1px;
}
</style>
