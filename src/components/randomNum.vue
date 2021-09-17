<template>
  <h1>随机数生成</h1>
  <li id="numInput" v-for="input in inputs" :key="input">
    <el-row>
      <el-col :span="8"></el-col>
      <el-col :span="2">
        <p>{{ input.str }}：</p>
      </el-col>
      <el-col :span="4">
        <el-input
          type="number"
          v-model="input.num"
          placeholder="请输入内容"
          maxlength="6"
        ></el-input>
      </el-col>
      <el-col :span="2"></el-col>
      <el-col :span="8"></el-col>
    </el-row>
  </li>
  <div>
    <el-radio v-model="radio" label="1">唯一</el-radio>
    <el-radio v-model="radio" label="2">不唯一</el-radio>
  </div>
  <el-button type="primary" @click="getResult()">生成generate</el-button>
  <el-button type="primary" @click="getResultz(10)"
    >生成一个1~10的随机数</el-button
  >
  <el-button type="primary" @click="getResultz(100)"
    >生成一个1~100的随机数</el-button
  >
  <p>{{ result }}</p>
</template>

<script>
import { defineComponent, ref } from "vue";
import { ElMessage } from "element-plus";

export default defineComponent({
  data() {
    return {
      inputs: [
        { str: "数量", num: ref("") },
        { str: "最小值", num: ref("") },
        { str: "最大值", num: ref("") },
      ],
      result: ref(""),
      radio: "1",
    };
  },
  methods: {
    RandomList(num, min, max) {
      if (min >= max) {
        ElMessage.error({
          message: "最小值应当比最大值小",
          type: "error",
        });
        return;
      }
      var range = max - min + 1;
      if (num > range) {
        ElMessage.error({
          message: "数量应小于或等于最大值减去最小值+1",
          type: "error",
        });
        return;
      }
      var points = [];
      for (var i = 0; i < range; i++) {
        points.push(min + i);
      }
      points.sort(function () {
        return 0.5 - Math.random();
      });
      return points.slice(0, num);
    },

    RandomListN(num, min, max) {
      if (min >= max) {
        ElMessage.error({
          message: "最小值应当比最大值小",
          type: "error",
        });
        return;
      }
      var range = max - min;
      var points = [];

      for (var i = 0; i < num; i++) {
        points.push(min + Math.round(Math.random() * range));
      }

      return points;
    },

    getResult() {
        // console.log(this.inputs[0].num);
        // console.log(this.inputs[0].num.indexOf("."));
      if (
        (this.inputs[0].num == "") |
        (this.inputs[1].num == "") |
        (this.inputs[2].num == "")
      ) {
        ElMessage.error({
          message: "输入框不能为空",
          type: "error",
        });
        return;
      } else if (
        this.inputs[0].num.indexOf(".") != -1 |
        this.inputs[1].num.indexOf(".") != -1 |
        this.inputs[2].num.indexOf(".") != -1
      ) {
        ElMessage.error({
          message: "数值只能为整数",
          type: "error",
        });
        return;
      } else {
        if (Number(this.inputs[0].num) <= 0) {
          ElMessage.error({
            message: "数量必须为正整数",
            type: "error",
          });
          return;
        }
        if (this.radio == "1") {
          this.result = this.RandomList(
            Number(this.inputs[0].num),
            Number(this.inputs[1].num),
            Number(this.inputs[2].num)
          );
          //   console.log(this.result[0]);
        } else {
          this.result = this.RandomListN(
            Number(this.inputs[0].num),
            Number(this.inputs[1].num),
            Number(this.inputs[2].num)
          );
        }
      }
    },
    getResultz(max) {
      this.result = this.RandomList(Number(1), Number(1), Number(max));
    },
  },
});
</script>

<style>
#numInput {
  list-style: none;
}
</style>