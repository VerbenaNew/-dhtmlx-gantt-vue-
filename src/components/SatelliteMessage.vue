<template>
  <div class="satellite-message">
    <el-card class="box-card">
      <div slot="header" class="clear-fix">
        <span>参数信息</span>
        <el-button
          style="float: right; padding: 3px 0"
          type="text"
          @click="closeMessage"
        >
          <i class="el-icon-close"></i>
        </el-button>
      </div>
      <div>
        <el-form ref="form" :model="detailForm" label-width="80px" size="mini">
          <el-form-item label="名称：">
            <label>{{ detailForm.name }}</label>
          </el-form-item>
          <div>
            <el-button size="mini" @click="showTable">表格数据</el-button>
            <el-button size="mini">表格数据</el-button>
          </div>
        </el-form>
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
      detailForm: {
        name: "名称",
      },
    };
  },
  mounted() {},
  methods: {
    showTable() {
      this.$emit("showTable", true);
    },
    closeMessage() {
      this.$emit("closeMessage", true);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style lang="scss" scoped>
.satellite-message {
  position: absolute;
  top: 50px;
  right: 30px;
  width: 500px;
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
