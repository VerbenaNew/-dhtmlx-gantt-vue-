<template>
  <div class="gantt-view">
    <div ref="gantt" class="ganter-content" />
  </div>
</template>

<script>
import { gantt } from "dhtmlx-gantt";
export default {
  name: "ganttView",
  props: {},
  data() {
    return {
      tasksData: {},
    };
  },
  mounted() {
    this.organizeSatellite();
  },
  methods: {
    /**
     * @description:先组织甘特图所需的数据
     */
    organizeSatellite() {
      this.tasksData = {
        data: [
          {
            id: 1,
            text: "分类1",
            start_date: "2020-12-15",
            processName: "阶段名",
            duration: 3,
            progress: 0.6,
            color: "#1890FF",
            type: "class",
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
            text: "名称",
            start_date: "2020-12-21",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            parent: 1,
            type: "task",
          },
          {
            id: 8,
            text: "名称",
            start_date: "2020-12-21",
            processName: "阶段名",
            duration: 3,
            progress: 0,
            color: "#1890FF ",
            parent: 2,
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
      this.initGunter();
    },
    /**
     * @description:初始化甘特图
     */
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
      gantt.config.show_links = false;
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
      // 时间线插件
      gantt.plugins({
        drag_timeline: true,
      });
      // 拖动时间线
      gantt.config.drag_timeline = {
        ignore: ".gantt_task_line, .gantt_task_link",
        useKey: false,
      };
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
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style lang="scss" scoped>
@import "~dhtmlx-gantt/codebase/dhtmlxgantt.css";

.gantt-view {
  position: relative;
  height: 500px;
}

.ganter-content {
  height: 100%;
}
</style>
