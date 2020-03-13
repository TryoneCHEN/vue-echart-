<template>
    <div class="hello">
        <div ref="mapbox" style="height:600px;width:800px"></div>
    </div>
</template>

<script>
import echarts from "echarts";
import "echarts/map/js/china.js";
import jsonp from "jsonp";
// 指定图表的配置项和数据
// let option = {
//     title: {
//         text: "ECharts 入门示例"
//     },
//     tooltip: {},
//     legend: {
//         data: ["销量"]
//     },
//     xAxis: {
//         data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
//     },
//     yAxis: {},
//     series: [
//         {
//             name: "销量",
//             type: "bar",
//             data: [5, 20, 36, 10, 10, 20]
//         }
//     ]
// };

// 数据
let option = {
    title: {
        text: "地图",
        link: "https://baidu.com",
        subtext: "你好",
        sublink: "https://baidu.com"
    },
    series: [
        {
            name: "确诊人数",
            type: "map",
            map: "china",
            label: {
                show: true,
                // color:"red",
                fontSize: 10
            },
            zoom: 1.2,
            roam:true,
            itemStyle: {
                // areaColor:'green'
                borderColor: "blue"
            },
            emphasis: {
                label: {
                    color: "#fff",
                    fontSize: 12
                },
                itemStyle: {
                    areaColor: "#83b5e7"
                }
            },
            data: []
        }
    ],
    toolbox: {
        show: true,
        orient: "vertical",
        left: "right",
        top: "center",
        feature: {
            // dataView: { readOnly: false },
            // restore: {},
            saveAsImage: {}
        }
    },
    visualMap: [
        {
            // 第一个 visualMap 组件
            type: "piecewise", // 定义为连续型 visualMap
            // show:false
            pieces: [
                { min: 10000 },
                { min: 1000, max: 9999 },
                { min: 100, max: 999 },
                { min: 10, max: 99 },
                { min: 1, max: 9 }
            ],
            inRange: {
                symbol: "rect",
                color: ["#ffc0b1", "#9c0505"]
            },
            itemWidth: 30,
            itemHeight: 20
        }
    ],
    tooltip: {
        trigger: "item"
    }
};
export default {
    name: "HelloWorld",
    mounted() {
        this.getData();
        // 基于准备好的dom，初始化echarts实例
        this.myChart = echarts.init(this.$refs.mapbox);
        this.myChart.setOption(option);
    },
    methods: {
        getData() {
            jsonp(
                "https://interface.sina.cn/news/wap/fymap2020_data.d.json?=1580892522427",
                {},
                (err, data) => {
                    if (!err) {
                        console.log(data);
                        option.series[0].data = data.data.list.map(item => ({
                            name: item.name,
                            value: item.value
                        }));
                        this.myChart.setOption(option);
                    }
                }
            );
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
    margin: 40px 0 0;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    display: inline-block;
    margin: 0 10px;
}
a {
    color: #42b983;
}
</style>
