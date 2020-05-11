<template>
  <div class="hello">
    <div ref="echarts" style="height:400px;width:600px"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import "echarts/map/js/china";
import jsonp from "jsonp";

let options = {
  title: {
    text: "新型冠状病毒肺炎"
  },
  series: [
    {
      type: "map",
      name: "确诊人数",
      map: "china",
      label: {
        // 地区文字
        show: true,
        color: "#000",
        fontSize: 13
      },
      zoom: 1.2,
      itemStyle: {
        // 地图板块
        areaColor: "#eee",
        borderColor: "#ccc"
      },
      emphasis: {
        //鼠标滑过
        label: {
          // 地区文字
          show: true,
          color: "#fff",
          fontSize: 16
        },
        itemStyle: {
          areaColor: "#83b5e7"
        }
      },
      roam: true,
      data: []
    }
  ],
  // 视觉映射
  visualMap: [
    {
      type: "piecewise",
      show: true,
      pieces: [
        { min: 10000 }, // 不指定 max，表示 max 为无限大（Infinity）。
        { min: 1000, max: 9999 },
        { min: 100, max: 999 },
        { min: 10, max: 999 },
        { min: 1, max: 9 }
      ],
      align: "right",
      inRange: {
        symbol: "rect",
        color: ["#ffc0b1", "#9c0505"]
      }
    }
  ],
  // 滑动显示人数
  tooltip: {
    trigger: "item"
  },
  // 下载
  toolbox: {
    show: true,
    orient: "vertical",
    left: "right",
    top: "center",
    feature: {
      dataView: { readOnly: false },
      restore: {},
      saveaSsImage: {}
    }
  }
};

export default {
  name: "echarts",
  data() {
    return {
      myechart: ""
    };
  },
  methods: {
    getinfo() {
      jsonp(
        "https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427&callback=__jp0",
        {},
        (err, data) => {
          if (!err) {
            let list = data.data.list.map(item => ({
              name: item.name,
              value: item.value
            }));
            options.series[0].data = list;
            this.myechart.setOption(options);
          }
        }
      );
    }
  },
  mounted() {
    this.getinfo();
    this.myechart = echarts.init(this.$refs.echarts);
    this.myechart.setOption(options);
  }
};
</script>

<style scoped lang="scss">
</style>
