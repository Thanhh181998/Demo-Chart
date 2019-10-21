<template>
  <div>
    <!-- <div class="avg-value">
      <span>{{title}}</span>
    </div> -->
    <!-- <VueApexCharts
      height="220"
      type="donut"
      :options="options"
      :series="series"
    ></VueApexCharts> -->
     <div class="donut-chart" ref="flushDistribution" style="height: 212px;width: 100%;"></div>
  </div>
</template>

<script>
//import VueApexCharts from "vue-apexcharts";
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

export default {
  name: "DonutChart",
  components: {
    //VueApexCharts
  },
  props: ["dataFlush"],
  mounted() {
    // let series = [];
    // series[0] = this.dataFlush.highFlushes;
    // series[1] = this.dataFlush.lowFlushes;
    // series[2] = this.dataFlush.userTriggered;
    // this.series = series;

    // const text = `${this.dataFlush.avgFlushesPerFixture} Avg Flushes per Fixture`;
    // //this.options.title.text = text;
    // this.title = text;

    let chart = am4core.create(this.$refs.flushDistribution, am4charts.PieChart);
    chart.data = [
      { "sector": "User Triggered", "top": "User", "bot": "Triggered", "size": 6.6},
      { "sector": "Low Flushes", "top": "Low", "bot": "Flushes", "size": 8.6},
      { "sector": "High Flushes", "top": "High", "bot": "Flushes", "size": 23.2},
    ];

    chart.responsive.enabled = true;
    chart.autoMargins = false;
    
    chart.innerRadius = 60;
    let label = chart.seriesContainer.createChild(am4core.Label);
    label.html = `<div><div style="font-size: 30px;text-align:center"><strong>132</strong></div><div style="margin-top: -20px;text-align:center"><span style="font-size: 9px;">Avg. Flushers</span></div><div style="margin-top: -32px;text-align:center"><span style="font-size: 9px;">per Fixture</span></div></div>`;
    label.horizontalCenter = "middle";
    label.verticalCenter = "middle";
    label.fontSize = 30;

    let pieSeries = chart.series.push(new am4charts.PieSeries());
    pieSeries.dataFields.value = "size";
    pieSeries.dataFields.category = "sector";
    pieSeries.dataFields.top = "top";
    pieSeries.dataFields.bot = "bot";
    pieSeries.labels.template.html = `
      <div style="float:left; font-size: 7px; margin-top: 3px"><span>{top}</span><br><span>{bot}</span></div>
	    <div style="font-size: 18px;margin-left: 30px;">
		    <strong>{value.percent.formatNumber('#.0')}%</strong>
      </div>
    `;
    //pieSeries.labels.template.htmlContainer = '300';

    pieSeries.slices.template.stroke = am4core.color("#fff");
    pieSeries.slices.template.strokeWidth = 7;
    pieSeries.slices.template.strokeOpacity = 1;
    pieSeries.colors.list = [
      am4core.color("#F3C7B2"),
      am4core.color("#FDDFB1"),
      am4core.color("#EA9D77"),
    ]
  },
  beforeDestroy() {
    if (this.chart) {
      this.chart.dispose();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
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

.avg-value {
  span{
    font-size: 13px;
  }
  margin-left: 3px;
}

</style>