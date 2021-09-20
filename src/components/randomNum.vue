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
    <el-radio v-model="littleNum" label="1">整数</el-radio>
    <el-radio v-model="littleNum" label="2">小数</el-radio>
  </div>
  <div>
    <el-row id="buf_hi" v-show="this.littleNum == '1' ? false : true">
      <el-col :span="5"></el-col>
      <el-col :span="5">
        <p>保留小数部分的位数：</p>
      </el-col>
      <el-col :span="4">
        <el-input
          type="number"
          v-model="littleLen"
          placeholder="请输入内容"
          maxlength="6"
        ></el-input>
      </el-col>
      <!-- <el-col :span="2"></el-col> -->
      <el-col :span="2"><p>(1~10)</p></el-col>
      <el-col :span="8"></el-col>
    </el-row>
  </div>

  <div id="buf_hi" v-show="this.littleNum == '1' ? true : false">
    <el-radio v-model="only" label="1">唯一</el-radio>
    <el-radio v-model="only" label="2">不唯一</el-radio>
    <!-- <div id='buf_hi'></div> -->
  </div>

  <el-button type="primary" @click="getResult()">生成generate</el-button>
  <el-button type="primary" @click="getResultz(100)"
    >生成一个0~100的随机整数</el-button
  >
  <el-button type="primary" @click="getResultl(100)"
    >生成一个0~100的随机小数</el-button
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
      littleNum: "1",
      only: "1",

      littleLen: ref(""),
    };
  },
  methods: {
    RandomLitList(num, min, max, num_len) {
      if (min >= max) {
        ElMessage.error({
          message: "最小值应当比最大值小",
          type: "error",
        });
        return;
      }
      var points = [];
      var range = max - min;
      for (var i = 0; i < num; i++) {
        points.push(Number((min + Math.random() * range).toFixed(num_len)));
      }
      return points;
    },
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
      }
      // else if (
      //   (this.inputs[0].num.indexOf(".") != -1) |
      //   (this.inputs[1].num.indexOf(".") != -1) |
      //   (this.inputs[2].num.indexOf(".") != -1)
      // ) {
      //   ElMessage.error({
      //     message: "数值只能为整数",
      //     type: "error",
      //   });
      //   return;
      // }
      else {
        if (Number(this.inputs[0].num) <= 0 | (this.inputs[0].num.indexOf(".") != -1)) {
          ElMessage.error({
            message: "数量必须为正整数",
            type: "error",
          });
          return;
        }
        if (this.littleNum == "2") {
          if (this.littleLen == "") {
            ElMessage.error({
              message: "输入框不能为空",
              type: "error",
            });
            return;
          } else if (
            (Number(this.littleLen) > 10) |
            (Number(this.littleLen) < 1) |
            (this.littleLen.indexOf(".") != -1)
          ) {
            ElMessage.error({
              message: "小数部分保留位数错误",
              type: "error",
            });
            return;
          }
          this.result = this.RandomLitList(
            Number(this.inputs[0].num),
            Number(this.inputs[1].num),
            Number(this.inputs[2].num),
            Number(this.littleLen)
          );
          return;
        }
        if (
          (this.inputs[0].num.indexOf(".") != -1) |
          (this.inputs[1].num.indexOf(".") != -1) |
          (this.inputs[2].num.indexOf(".") != -1)
        ) {
          ElMessage.error({
            message: "数值只能为整数",
            type: "error",
          });
          return;
        }
        if (this.only == "1") {
          this.result = this.RandomList(
            Number(this.inputs[0].num),
            Number(this.inputs[1].num),
            Number(this.inputs[2].num)
          );
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
      this.result = this.RandomList(Number(1), Number(0), Number(max));
    },
    getResultl(max) {
      this.result = this.RandomLitList(Number(1), Number(0), Number(max), 10);
    },
  },
});
</script>

<style>
#numInput {
  list-style: none;
}
#buf_hi {
  height: 53px;
}
</style>