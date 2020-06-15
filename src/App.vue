<template>
  <div id="app">
    <div id="controls"></div>
    <div class="hello" id="chartdiv"></div>

    <div class="hello" id="pieChart"></div>
    <div class="hello" id="barChart"></div>
  </div>
</template>

<script src="https://www.amcharts.com/lib/4/plugins/rangeSelector.js"></script>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
import am4themes_material from "@amcharts/amcharts4/themes/material";
import * as am4plugins_rangeSelector from "@amcharts/amcharts4/plugins/rangeSelector";
import { any } from "@amcharts/amcharts4/.internal/core/utils/Array";

am4core.useTheme(am4themes_animated);
am4core.useTheme(am4themes_material);

export default {
  name: "App",
  tempBatChart: any,
  mounted() {
    // let chart = am4core.create(this.$refs.chartdiv, am4charts.XYChart);

    // chart.paddingRight = 20;

    // let data = [];
    // let visits = 10;
    // for (let i = 1; i < 366; i++) {
    //   visits += Math.round((Math.random() < 0.5 ? 1 : -1) * Math.random() * 10);
    //   data.push({
    //     date: new Date(2018, 0, i),
    //     name: "name" + i,
    //     value: visits
    //   });
    // }

    // chart.data = data;

    // let dateAxis = chart.xAxes.push(new am4charts.DateAxis());
    // dateAxis.renderer.grid.template.location = 0;

    // let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
    // valueAxis.tooltip.disabled = true;
    // valueAxis.renderer.minWidth = 35;

    // let series = chart.series.push(new am4charts.LineSeries());
    // series.dataFields.dateX = "date";
    // series.dataFields.valueY = "value";

    // series.tooltipText = "{valueY.value}";
    // chart.cursor = new am4charts.XYCursor();

    // let scrollbarX = new am4charts.XYChartScrollbar();
    // scrollbarX.series.push(series);
    // chart.scrollbarX = scrollbarX;

    // this.chart = chart;

    let chart = am4core.create("chartdiv", am4charts.XYChart);
    chart.padding(0, 15, 0, 15);
    chart.colors.step = 3;

    let data = [];
    let price1 = 1000;
    let price2 = 2000;
    let price3 = 3000;
    let quantity = 1000;
    for (var i = 15; i < 3000; i++) {
      price1 += Math.round(
        (Math.random() < 0.5 ? 1 : -1) * Math.random() * 100
      );
      price2 += Math.round(
        (Math.random() < 0.5 ? 1 : -1) * Math.random() * 100
      );
      price3 += Math.round(
        (Math.random() < 0.5 ? 1 : -1) * Math.random() * 100
      );

      if (price1 < 100) {
        price1 = 100;
      }

      if (price2 < 100) {
        price2 = 100;
      }

      if (price3 < 100) {
        price3 = 100;
      }

      quantity += Math.round(
        (Math.random() < 0.5 ? 1 : -1) * Math.random() * 500
      );

      if (quantity < 0) {
        quantity *= -1;
      }
      data.push({
        date: new Date(2000, 0, i),
        price1: price1,
        price2: price2,
        price3: price3,
        quantity: quantity
      });
    }

    chart.data = data;
    // the following line makes value axes to be arranged vertically.
    chart.leftAxesContainer.layout = "vertical";

    // uncomment this line if you want to change order of axes
    //chart.bottomAxesContainer.reverseOrder = true;

    let dateAxis = chart.xAxes.push(new am4charts.DateAxis());
    dateAxis.renderer.grid.template.location = 0;
    dateAxis.renderer.ticks.template.length = 8;
    dateAxis.renderer.ticks.template.strokeOpacity = 0.1;
    dateAxis.renderer.grid.template.disabled = true;
    dateAxis.renderer.ticks.template.disabled = false;
    dateAxis.renderer.ticks.template.strokeOpacity = 0.2;
    dateAxis.renderer.minLabelPosition = 0.01;
    dateAxis.renderer.maxLabelPosition = 0.99;
    dateAxis.keepSelection = true;

    dateAxis.groupData = true;
    dateAxis.minZoomCount = 5;

    // these two lines makes the axis to be initially zoomed-in
    // dateAxis.start = 0.7;
    // dateAxis.keepSelection = true;

    let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
    valueAxis.tooltip.disabled = true;
    valueAxis.zIndex = 1;
    valueAxis.renderer.baseGrid.disabled = true;
    // height of axis
    valueAxis.height = am4core.percent(65);

    valueAxis.renderer.gridContainer.background.fill = am4core.color("#000000");
    valueAxis.renderer.gridContainer.background.fillOpacity = 0.05;
    valueAxis.renderer.inside = true;
    valueAxis.renderer.labels.template.verticalCenter = "bottom";
    valueAxis.renderer.labels.template.padding(2, 2, 2, 2);

    //valueAxis.renderer.maxLabelPosition = 0.95;
    valueAxis.renderer.fontSize = "0.8em";

    let series1 = chart.series.push(new am4charts.LineSeries());
    series1.dataFields.dateX = "date";
    series1.dataFields.valueY = "price1";
    series1.dataFields.valueYShow = "changePercent";
    series1.tooltipText =
      "{name}: {valueY.changePercent.formatNumber('[#0c0]+#.00|[#c00]#.##|0')}%";
    series1.name = "Stock A";
    series1.tooltip.getFillFromObject = false;
    series1.tooltip.getStrokeFromObject = true;
    series1.tooltip.background.fill = am4core.color("#fff");
    series1.tooltip.background.strokeWidth = 2;
    series1.tooltip.label.fill = series1.stroke;

    let series2 = chart.series.push(new am4charts.LineSeries());
    series2.dataFields.dateX = "date";
    series2.dataFields.valueY = "price2";
    series2.dataFields.valueYShow = "changePercent";
    series2.tooltipText =
      "{name}: {valueY.changePercent.formatNumber('[#0c0]+#.00|[#c00]#.##|0')}%";
    series2.name = "Stock B";
    series2.tooltip.getFillFromObject = false;
    series2.tooltip.getStrokeFromObject = true;
    series2.tooltip.background.fill = am4core.color("#fff");
    series2.tooltip.background.strokeWidth = 2;
    series2.tooltip.label.fill = series2.stroke;

    let series3 = chart.series.push(new am4charts.LineSeries());
    series3.dataFields.dateX = "date";
    series3.dataFields.valueY = "price3";
    series3.dataFields.valueYShow = "changePercent";
    series3.tooltipText =
      "{name}: {valueY.changePercent.formatNumber('[#0c0]+#.00|[#c00]#.##|0')}%";
    series3.name = "Stock C";
    series3.tooltip.getFillFromObject = false;
    series3.tooltip.getStrokeFromObject = true;
    series3.tooltip.background.fill = am4core.color("#fff");
    series3.tooltip.background.strokeWidth = 2;
    series3.tooltip.label.fill = series3.stroke;

    let valueAxis2 = chart.yAxes.push(new am4charts.ValueAxis());
    valueAxis2.tooltip.disabled = true;
    // height of axis
    valueAxis2.height = am4core.percent(35);
    valueAxis2.zIndex = 3;
    // this makes gap between panels
    valueAxis2.marginTop = 30;
    valueAxis2.renderer.baseGrid.disabled = true;
    valueAxis2.renderer.inside = true;
    valueAxis2.renderer.labels.template.verticalCenter = "bottom";
    valueAxis2.renderer.labels.template.padding(2, 2, 2, 2);
    //valueAxis.renderer.maxLabelPosition = 0.95;
    valueAxis2.renderer.fontSize = "0.8em";

    valueAxis2.renderer.gridContainer.background.fill = am4core.color(
      "#000000"
    );
    valueAxis2.renderer.gridContainer.background.fillOpacity = 0.05;

    let volumeSeries = chart.series.push(new am4charts.StepLineSeries());
    volumeSeries.fillOpacity = 1;
    volumeSeries.fill = series1.stroke;
    volumeSeries.stroke = series1.stroke;
    volumeSeries.dataFields.dateX = "date";
    volumeSeries.dataFields.valueY = "quantity";
    volumeSeries.yAxis = valueAxis2;
    volumeSeries.tooltipText = "Volume: {valueY.value}";
    volumeSeries.name = "Series 2";
    // volume should be summed
    volumeSeries.groupFields.valueY = "sum";
    volumeSeries.tooltip.label.fill = volumeSeries.stroke;
    chart.cursor = new am4charts.XYCursor();

    let scrollbarX = new am4charts.XYChartScrollbar();
    scrollbarX.series.push(series1);
    scrollbarX.marginBottom = 20;
    let sbSeries = scrollbarX.scrollbarChart.series.getIndex(0);
    sbSeries.dataFields.valueYShow = undefined;
    chart.scrollbarX = scrollbarX;

    // Add range selector
    let selector = new am4plugins_rangeSelector.DateAxisRangeSelector();
    selector.container = document.getElementById("controls");
    selector.axis = dateAxis;

    //////////////////////

    let pieChart = am4core.create("pieChart", am4charts.PieChart);

    // Add data
    pieChart.data = [
      { sector: "Agriculture", size: 6.6 },
      { sector: "Mining and Quarrying", size: 0.6 },
      { sector: "Manufacturing", size: 23.2 },
      { sector: "Electricity and Water", size: 2.2 },
      { sector: "Construction", size: 4.5 },
      { sector: "Trade (Wholesale, Retail, Motor)", size: 14.6 },
      { sector: "Transport and Communication", size: 9.3 },
      { sector: "Finance, real estate and business services", size: 22.5 }
    ];

    // Add label
    pieChart.innerRadius = 100;
    let label = pieChart.seriesContainer.createChild(am4core.Label);
    label.text = "1995";
    label.horizontalCenter = "middle";
    label.verticalCenter = "middle";
    label.fontSize = 50;

    // Add and configure Series
    let pieSeries = pieChart.series.push(new am4charts.PieSeries());
    pieSeries.dataFields.value = "size";
    pieSeries.dataFields.category = "sector";
    pieSeries.labels.template.disabled = true;

    // Animate chart data
    let currentYear = 1995;
    function getCurrentData() {
      label.text = currentYear;
      let data = pieChartData[currentYear];
      // currentYear++;
      // if (currentYear > 2014) currentYear = 1995;
      return data;
    }

    //////////////////////

    let barChart = am4core.create("barChart", am4charts.XYChart);

    barChart.data = [
      {
        year: 2005,
        income: 20,
        expenses: 25
      },
      {
        year: 2006,
        income: 22,
        expenses: 17
      },
      {
        year: 2007,
        income: 16,
        expenses: 19
      }
    ];

    let categoryAxis = barChart.yAxes.push(new am4charts.CategoryAxis());
    categoryAxis.dataFields.category = "year";
    categoryAxis.numberFormatter.numberFormat = "#";
    categoryAxis.renderer.inversed = true;
    categoryAxis.renderer.grid.template.location = 1;
    categoryAxis.renderer.cellStartLocation = 0.1;
    categoryAxis.renderer.cellEndLocation = 0.9;

    let valueAxisBarChart = barChart.xAxes.push(new am4charts.ValueAxis());
    valueAxisBarChart.renderer.opposite = true;
    valueAxisBarChart.renderer.cellStartLocation = 1;
    valueAxisBarChart.renderer.cellEndLocation = 0;

    ////
    // let series = barChart.series.push(new am4charts.ColumnSeries());
    // series.dataFields.valueX = "income";
    // series.dataFields.categoryY = "year";
    // series.name = "Income";
    // series.columns.template.tooltipText = "Income: [bold]{valueX}[/]";
    // series.columns.template.height = am4core.percent(100);
    // series.sequencedInterpolation = true;

    // let seriess = barChart.series.push(new am4charts.ColumnSeries());
    // seriess.dataFields.valueX = "expenses";
    // seriess.dataFields.categoryY = "year";
    // seriess.name = "Expenses";
    // seriess.columns.template.tooltipText = "Expenses: [bold]{valueX}[/]";
    // seriess.columns.template.height = am4core.percent(100);
    // seriess.sequencedInterpolation = true;

    // let valueLabel = series.bullets.push(new am4charts.LabelBullet());
    // valueLabel.label.text = "{valueX}";
    // valueLabel.label.horizontalCenter = "left";
    // valueLabel.label.dx = 10;
    // valueLabel.label.hideOversized = false;
    // valueLabel.label.truncate = false;

    // let categoryLabel = series.bullets.push(new am4charts.LabelBullet());
    // categoryLabel.label.text = "Income";
    // categoryLabel.label.horizontalCenter = "right";
    // categoryLabel.label.dx = -10;
    // categoryLabel.label.fill = am4core.color("#fff");
    // categoryLabel.label.hideOversized = false;
    // categoryLabel.label.truncate = false;
    ////
    this.tempBatChart = barChart;
    this.createSeries("income", "Income");
    this.createSeries("expenses", "Expenses");
  },

  beforeDestroy() {
    if (this.chart) {
      this.chart.dispose();
    }
  },

  /////////////
  methods: {
    // Create series
    createSeries(field, name) {
      let series = this.tempBatChart.series.push(new am4charts.ColumnSeries());
      series.dataFields.valueX = field;
      series.dataFields.categoryY = "year";
      series.name = name;
      series.columns.template.tooltipText = "{name}: [bold]{valueX}[/]";
      series.columns.template.height = am4core.percent(100);
      series.sequencedInterpolation = true;

      // let valueLabel = series.bullets.push(new am4charts.LabelBullet());
      // valueLabel.label.text = "{valueX}";
      // valueLabel.label.horizontalCenter = "left";
      // valueLabel.label.dx = 10;
      // valueLabel.label.hideOversized = false;
      // valueLabel.label.truncate = false;

      // let categoryLabel = series.bullets.push(new am4charts.LabelBullet());
      // categoryLabel.label.text = "{name}";
      // categoryLabel.label.horizontalCenter = "right";
      // categoryLabel.label.dx = -10;
      // categoryLabel.label.fill = am4core.color("#fff");
      // categoryLabel.label.hideOversized = false;
      // categoryLabel.label.truncate = false;
    }
  }
};
</script>

<style scoped>
.hello {
  width: 100%;
  height: 500px;
}
</style>
