<template>
  <div class="gantt-view">
    <div class="legend-container">
      <el-radio-group v-model="radioTime">
        <el-radio :label="2">2小时</el-radio>
        <el-radio :label="4">4小时</el-radio>
        <el-radio :label="8">8小时</el-radio>
        <el-radio :label="12">12小时</el-radio>
        <el-radio :label="24">24小时</el-radio>
      </el-radio-group>
      <li
        v-for="(val, key, index) in legendData"
        :key="index"
        class="legend-item"
      >
        <span :style="{ background: val }" class="legend-color"></span>
        <span class="legend-text">{{ key }}</span>
      </li>
    </div>
    <div ref="gantt" class="ganter-content"></div>
    <SatelliteMessage
      v-if="detailModel"
      @showTable="showTable"
      @closeMessage="closeMessage"
    ></SatelliteMessage>
    <ParamTable v-if="tableModel" @closeTable="closeTable"></ParamTable>
  </div>
</template>

<script>
import SatelliteMessage from "./SatelliteMessage";
import ParamTable from "./ParamTable";

import { gantt } from "dhtmlx-gantt"; //依赖
import "dhtmlx-gantt/codebase/skins/dhtmlxgantt_terrace.css"; //皮肤
import "dhtmlx-gantt/codebase/dhtmlxgantt.css";
export default {
  name: "ganttView",
  props: {},
  components: {
    SatelliteMessage: SatelliteMessage,
    ParamTable: ParamTable,
  },
  data() {
    return {
      // 甘特图数据
      tasksData: {},
      // 时间间隔
      radioTime: 24,
      // 图例数据
      legendData: {
        阶段名: "#65c16f",
      },
      // 是否展示详情
      detailModel: false,
      // 是否展示参数对应的详细表格
      tableModel: false,
    };
  },
  mounted() {
    this.organizeSatellite();
  },
  methods: {
    // 展示表格数据
    showTable() {
      this.tableModel = true;
    },
    // 关闭表格框
    closeTable() {
      this.tableModel = false;
    },
    // 关闭信息框
    closeMessage() {
      this.detailModel = false;
    },
    // 先组织甘特图所需的数据
    organizeSatellite() {
      const tasksData = {
        data: [
          {
            id: 1,
            text: "分类1",
            // start_date: "2020-12-15",
            processName: "阶段名",
            // duration: 3,
            // progress: 0.6,
            color: "#1890FF ",
            type: "class",
            render: "split",
          },
          {
            id: 2,
            text: "分类2",
            start_date: "2020-12-18",
            processName: "阶段名",
            duration: 6,
            progress: 0.4,
            color: "#1890FF ",
            type: "class",
            render: "split",
          },
          {
            id: 3,
            text: "分类3",
            start_date: "2020-12-18",
            processName: "阶段名",
            duration: 4,
            progress: 0.2,
            color: "#1890FF ",
            type: "class",
            render: "split",
          },
          {
            id: 4,
            text: "分类4",
            start_date: "2020-12-21",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            type: "class",
            render: "split",
          },
          {
            id: 5,
            text: "名称",
            start_date: "2020-12-21",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            parent: 2,
            type: "task",
          },
          {
            id: 6,
            text: "名称",
            start_date: "2020-12-21",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            parent: 3,
            type: "task",
          },
          {
            id: 7,
            text: "名称1",
            start_date: "2020-12-21",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            parent: 1,
            type: "task",
            render: "split",
          },
          {
            id: 8,
            text: "名称2",
            start_date: "2020-12-25",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            parent: 7,
            type: "task",
          },
          {
            id: 9,
            text: "名称3",
            start_date: "2020-12-14",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            parent: 7,
            type: "task",
          },
          {
            id: 10,
            text: "名称4",
            start_date: "2020-12-31",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            parent: 7,
            type: "task",
          },
        ],
        links: [
          {
            id: 1,
            source: 1,
            target: 2,
            type: "0",
          },
        ],
      };
      this.tasksData.data = [];
      tasksData.data.forEach((el) => {
        let eli = this.organizeColor(el);
        this.tasksData.data.push(eli);
      });
      this.initGunter();
    },
    // 组织颜色--可以自定义每个任务阶段的颜色
    organizeColor(property) {
      let description = property;
      switch (property.processName) {
        case "阶段名":
          description.color = "#65c16f";
          break;
        default:
          description.color = "#f6f6f6";
          break;
      }
      return description;
    },
    // 初始化甘特图
    initGunter() {
      // 甘特图只读
      gantt.config.readonly = true;
      // 配置左侧树
      gantt.config.columns = [
        {
          name: "text",
          label: "名称",
          width: 217,
          tree: true, // 如果有子元素是否遍历
          align: "center",
          template: function (obj) {
            return `${obj.text}`; // 通过 template 回调可以指定返回内容值
          },
        },
      ];
      // 配置右侧头部
      //   gantt.config.subscales = [
      //     {
      //       unit: 'day',
      //       step: 4,
      //       date: '%Y/%m/%d'
      //     }
      //   ]
      gantt.config.autoscroll = true;
      //   gantt.config.autosize = 'xy'
      // 设置表头的高度
      gantt.config.scale_height = 66;
      //   gantt.config.duration_unit = 'hour'
      //   gantt.config.duration_step = 6
      //   gantt.config.date_scale = '%H: %i'
      // 是否显示依赖连线
      gantt.config.show_links = true;
      // 隐藏所有标记
      gantt.config.show_markers = true;
      gantt.config.xml_date = "%Y-%m-%d";
      // 表头设置
      gantt.config.scales = [
        {
          unit: "day",
          step: 1,
          format: "%Y/%m/%d",
        },
        {
          unit: "hour",
          step: 1,
          format: "%H:%i:%s",
        },
      ];
      // 添加tooltip提示框
      gantt.plugins({
        tooltip: true,
        lightbox: true,
      });
      // // 添加灯箱属性
      // gantt.config.lightbox.sections = [
      //   {
      //     name: "description",
      //     height: 70,
      //     map_to: "text",
      //     type: "textarea",
      //     focus: true,
      //   },
      //   { name: "type", type: "typeselect", map_to: "type" },
      //   { name: "time", type: "duration", map_to: "auto" },
      // ];
      // 时间线插件
      gantt.plugins({
        drag_timeline: true,
      });
      // 拖动时间线
      gantt.config.drag_timeline = {
        ignore: ".gantt_task_line, .gantt_task_link",
        useKey: false,
      };
      //
      gantt.config.open_split_tasks = true;
      // task 自定义样式
      // gantt.templates.task_class = function (start, end, task) {
      //   console.log(start, end, task);
      //   switch (task.processName) {
      //     case "阶段名":
      //       return "process-column";
      //     // break;
      //     case "task":
      //       return "task-task";
      //     // break;
      //   }
      // };
      // 初始化
      gantt.init(this.$refs.gantt);
      // 绘制甘特图
      gantt.parse(this.tasksData);
      const that = this;
      // 添加事件处理器需要用到attachEvent方法
      // 左侧栏的点击事件
      // gantt.attachEvent("onTaskRowClick", (id, e) => {
      // var domHelpers = gantt.utils.dom;
      // if (
      //   !domHelpers.closest(e.target, "[" + gantt.config.link_attribute + "]")
      // ) {
      //   gantt.message("not a link");
      // } else {
      //   gantt.message("link!");
      // }
      // });
      // 任务栏点击事件
      gantt.attachEvent("onTaskClick", (id, e) => {
        let taski = gantt.getTask(id);
        console.log(taski);
        if (taski.$open) {
          gantt.close(id);
        } else {
          gantt.open(id);
        }
        that.taskClick(id, e);
      });
      //移除某事件处理器
      // gantt.detachEvent("onTaskClick");
      // 移除所有事件处理
      // gantt.detachAllEvents();
      // 检查某个事件是否添加了事件处理器
      // gantt.checkEvent("onTaskClick"); //returns 'true'
    },
    // 单个任务单击事件
    taskClick(id) {
      this.orbitBySatellite(id);
    },
    // 根据卫星id绘制轨道
    orbitBySatellite(id) {
      const satellite = id;
      console.log(satellite);
      this.detailModel = true;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style lang="scss" scoped>
.gantt-view {
  position: relative;
  height: 500px;
}
.legend-container {
  display: inline-block;
  text-align: right;
  padding: 0 20px;
  line-height: 50px;
}
.legend-container li {
  list-style: none;
  display: inline-block;
}
.legend-item {
  margin: 0 5px;
}
.legend-color {
  margin: 0 5px;
  display: inline-block;
  height: 20px;
  width: 30px;
  vertical-align: middle;
}
.legend-text {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.ganter-content {
  height: 400px;
}
</style>
