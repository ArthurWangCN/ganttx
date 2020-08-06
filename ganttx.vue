<template>
  <div ref="gantt"></div>
</template>

<script>
/*
  * Rely on DHTMLX Gantt
  * "dhtmlx-gantt": "^7.0.8"
  * Powered By ArthurWang77
  * Contact me: arthurwang77@qq.com
  * CN Documents: https://blog.csdn.net/qq_24472595/article/details/81630117?utm_source=blogxgwz1
*/
import { gantt } from 'dhtmlx-gantt';
export default {
  name: 'gantt',
  data() {
    return {
      newObj: {}
    }
  },
  props: {
    tasks: {
      type: Object,
      default () {
        return {data: [], links: []}
      }
    }
  },

  created() {
  },

  mounted: function () {
    // 初始化甘特图配置
    this.initConfig();

    // 甘特图初始化和导入数据
    gantt.init(this.$refs.gantt);
    gantt.parse(this.tasks);
  },

  methods: {
    // 初始化甘特图数据
    initData() {

    },
    // 初始化甘特图配置
    initConfig() {
      // 1 基础配置
      // 1.1 甘特图是否只读
      gantt.config.readonly = true;
      // 1.2 表格列设置
      gantt.config.columns = [
        {name:"text", label:"任务名", tree:true, width:'200' },
        // {name:"start_date", label:"开始时间", align:"center", width:'*' },
        // {name:"end_date", label:"结束时间", align:"center" , width:'*'},
        // {name:"duration",   label:"Duration",   align: "center" },
      ];
      // 1.3 设置中文
      gantt.i18n.setLocale("cn");
      // 1.4 鼠标悬浮框
      gantt.plugins({tooltip: true});// 启用tooltip悬浮框
      gantt.templates.tooltip_text = function(start, end, task) {
        // return "<b>任务名称:</b> "+task.text+"<br/><b>时长:</b> " + task.duration+"天<br/><b>说明:</b>" +task.toolTipsTxt;
        return "<b>任务名称:</b> "+task.text+"<br/><b>开始时间:</b> " + gantt.templates.tooltip_date_format(start)+"<br/><b>结束时间:</b>" +gantt.templates.tooltip_date_format(end);
      }
      // 1.5 初始化的时候就展开树结构
      gantt.config.open_tree_initially = true;
      // 1.6 显示到任务上的文本
      gantt.templates.task_text = function (start, end, task) {
        // return "<b style='text-align:left;'>计划名称:</b> " + task.text +"    <span style='text-align:left;'>" +" 完成计划："+ Math.round(task.progress * 100) + "% </span>";
        return "<b style='text-align:left;'>任务名称:</b> " + task.text;
      };
      // 1.7 显示单元格
      gantt.config.show_task_cells = true;
      // 1.8 自适应甘特图的尺寸大小
      gantt.config.autosize = 'y';
      // 1.9 标记今天
      gantt.plugins({marker: true});// 启用marker插件
      this.addTodayLine();

      // 2 时间配置
      // 2.1 时间坐标轴单位 minute/hour/day/week/quarter/month/year
      gantt.config.scale_unit = "day";
      // 2.2 日期格式
      gantt.config.date_scale = "%d";
      // 2.3 时间坐标为月份的时候  先显示年份再月份
      gantt.config.subscales = [{unit: "month", step: 1, date: "%Y,%F"}];
      // 2.4 定义从后端获取或发送到后端的日期数据解析格式
      gantt.config.xml_date = "%Y-%m-%d";

      // 3 拖动配置
      // 3.1 自动调整类型,当存在子节点时自动升级为project
      gantt.config.auto_types = false;
      // 3.2 设置不可以拖动进度
      gantt.config.drag_progress = true;
      // 3.3 设置Task不可以拖动
      gantt.config.drag_move = false;
      // 3.4 设置不可以拖动关系
      gantt.config.drag_links = true;
      // 3.5 设置不可拖动Task 大小
      gantt.config.drag_resize = true;
      // 3.6 单击显示添加详情
      gantt.config.details_on_create = true;
      // 3.7 双击显示明细
      gantt.config.details_on_dblclick = true;
      // 3.8 timeline区域是否自动初始化滚动, 用于显示更早之前的任务
      gantt.config.initial_scroll = false;

    },
    // 重新加载
    reload() {
      gantt.clearAll();
      this.addTodayLine();
      gantt.parse(this.$props.tasks);
      gantt.render();
    },
    // 添加今日标记线
    addTodayLine() {
      var date_to_str = gantt.date.date_to_str("%Y-%m-%d");
      var start = new Date();
      gantt.addMarker({
        start_date: start,
        css: "status_line",
        text: "今天",
        title: "今天: " + date_to_str(start)
      });
    }
  }
}
</script>

<style>
  @import "~dhtmlx-gantt/codebase/dhtmlxgantt.css";
  /* 任务名前的logo */
  .gantt_tree_icon.gantt_file, .gantt_tree_icon.gantt_folder_open, .gantt_tree_icon.gantt_folder_closed{width: 10px; background-image: none;}
  .gantt_tree_content{overflow: hidden;text-overflow: ellipsis;}
</style>
