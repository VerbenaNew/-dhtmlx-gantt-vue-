<template>
  <div class="param-table">
    <el-card class="box-card">
      <div slot="header" class="clear-fix">
        <span>参数详情</span>
        <el-button
          style="float: right; padding: 3px 0"
          type="text"
          @click="closeTable"
        >
          <i class="el-icon-close"></i>
        </el-button>
      </div>
      <div>
        <el-table :data="tableData" height="500" border style="width: 100%">
          <el-table-column prop="time" label="时间" width="180">
          </el-table-column>
          <el-table-column prop="name" label="参数名" width="180">
          </el-table-column>
          <el-table-column prop="detail" label="详细信息"> </el-table-column>
        </el-table>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "SatelliteMessage",
  props: {},
  data() {
    return {
      tableData: [
        {
          time: "2016-05-03",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-02",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-04",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-01",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-08",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-06",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-07",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-09",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-17",
          name: "参数名",
          detail: "详细信息",
        },
        {
          time: "2016-05-18",
          name: "参数名",
          detail: "详细信息",
        },
      ],
    };
  },
  mounted() {},
  methods: {
    closeTable() {
      this.$emit("closeTable", false);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style lang="scss" scoped>
.param-table {
  position: absolute;
  top: 50px;
  left: 30px;
  width: 600px;
}
.clear-fix {
  text-align: left;
}
.clear-fix:before,
.clear-fix:after {
  display: table;
  content: "";
}
.clear-fix:after {
  clear: both;
}

.box-card {
  width: 100%;
}
</style>
