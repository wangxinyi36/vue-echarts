<template>
  <div class="container">
    <el-radio-group v-model="radio" @change="handleChange">
      <el-radio-button label="week">最近一周</el-radio-button>
      <el-radio-button label="month">最近一个月</el-radio-button>
      <el-radio-button label="half-year">最近半年</el-radio-button>
      <el-radio-button label="year">最近一年</el-radio-button>
      <el-radio-button label="other">自定义</el-radio-button>
    </el-radio-group>

    <div id="main" class="line-box"></div>
  </div>
</template>

<script>
import * as echarts from "echarts";
export default {
  data() {
    return {
      myChart: "",
      radio: "week",
      timeList: [],
    };
  },
  mounted() {
    let timeList = [];
    for (let i = 0; i <= 7; i++) {
      timeList.push(this.$dayjs().subtract(i, "day").format("YYYY-MM-DD"));
    }
    this.timeList = timeList;
    this.init();
  },
  methods: {
    //   初始化
    init() {
      let myChart = echarts.init(document.getElementById("main"));
      let option = {
        title: {
          text: "用户量",
        },
        tooltip: {
          trigger: "axis",
        },
        legend: {
          data: ["新增用户量", "总用户量"],
        },
        xAxis: {
          type: "category",
          data: this.timeList,
          axisLabel: {
            interval: 0,
            rotate: 45,
          },
        },
        yAxis: {},
        series: [
          {
            type: "line",
            name: "新增用户量",
            data: [23, 44, 55, 19, 44, 55, 19],
          },
          {
            type: "line",
            name: "总用户量",
            data: [13, 4, 95, 29, 34, 5, 69],
          },
        ],
      };
      myChart.setOption(option);
      this.myChart = myChart;
    },
    handleChange() {
      let timeList = [];
      if (this.radio == "week") {
        for (let i = 0; i <= 7; i++) {
          timeList.push(this.$dayjs().subtract(i, "day").format("YYYY-MM-DD"));
        }
      } else if (this.radio == "month") {
        let monthDs = this.$dayjs().daysInMonth(); //当前月份天数
        for (let i = 0; i <= monthDs; i++) {
          timeList.push(this.$dayjs().subtract(i, "day").format("YYYY-MM-DD"));
        }
      } else if (this.radio == "half-year") {
        for (let i = 0; i <= 6; i++) {
          timeList.push(this.$dayjs().subtract(i, "month").format("YYYY.MM"));
        }
      } else if (this.radio == "year") {
        for (let i = 0; i <= 12; i++) {
          timeList.push(this.$dayjs().subtract(i, "month").format("YYYY.MM"));
        }
      }
      this.timeList = timeList;
      this.myChart.setOption({
        xAxis: {
          data: this.timeList,
        },
      });
    },
  },
};
</script>
<style>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.line-box {
  width: 80%;
  height: 500px;
}
</style>