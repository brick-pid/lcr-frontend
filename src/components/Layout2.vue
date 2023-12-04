<style>
.layout{
  border: 1px solid #d7dde4;
  background: #f5f7f9;
  position: relative;
  border-radius: 4px;
  overflow: hidden;
}
.layout-logo{
  width: 150px;
  height: 30px;
  background: #5b6270;
  border-radius: 3px;
  float: left;
  position: relative;
  top: 10px;
  left: 20px;
}
/* .layout-nav{
  width: 420px;
  margin: 0 auto;
  margin-right: 20px;
} */
.layout-nav {
  /* Increase the width or make it flexible */
  width: auto;
  max-width: 100%; /* or a larger fixed width if necessary */
  margin: 0 auto;
  display: flex;         /* 使用 flexbox 布局 */
  justify-content: flex-end; /* 使内容靠右对齐 */
  align-items: center;   /* 如果需要，可以垂直居中对齐内容 */
  margin-right: 20px;    /* 根据需要保持或调整右边距 */
}
.layout{
  text-align: initial;
  padding: 24px;
  minHeight: 280px;
  background: #fff;
  overflow: auto;
}
.box{
  margin: 0 auto;
  padding-top: 10px;
}
</style>
<template>
  <div class="layout">
    <Layout>
      <Header>
        <Menu mode="horizontal" theme="dark" active-name="1">
          <div class="layout-logo">
            <h2 style="color: #f5f7f9;text-align: center;top:15px;font-family: 'Lucida Calligraphy', cursive, serif, sans-serif;">银行案例处罚库</h2>
          </div>
          <div class="layout-nav">
            <MenuItem name="1" to="/penalty">
              <Icon type="ios-navigate" ></Icon>
              违规案例库
            </MenuItem>
            <MenuItem name="2" to="/area">
              <Icon type="ios-paper"></Icon>
              区域分析
            </MenuItem>
            <MenuItem name="3" to="/trend">
              <Icon type="ios-analytics"></Icon>
              趋势分析
            </MenuItem>
            <!-- 新添加的菜单项 -->
            <MenuItem name="4" to="/case-search">
              <Icon type="ios-search"></Icon>
              类案检索
            </MenuItem>
          </div>
        </Menu>
      </Header>
      <Layout>
        <Sider hide-trigger :style="{background: '#fff'}">
          <el-menu active-name="1-2" theme="light" width="auto" :open-names="['1']" :router="true">
            <el-submenu index="1">
              <template #title>
                <Icon type="ios-navigate"></Icon>
                总体情况
              </template>
              <el-menu-item index="/penalty">违规案例库</el-menu-item>
              <el-menu-item  index="/global">总体处罚情况</el-menu-item>
            </el-submenu>
            <el-submenu index="2">
              <template #title>
                <Icon type="ios-analytics"></Icon>
                分析查询
              </template>
              <el-menu-item index="/area">全国区域分析</el-menu-item>
              <el-menu-item index="/decision">处罚决定分析</el-menu-item>
              <el-menu-item index="/violation">业务领域/违规点分析</el-menu-item>
              <el-menu-item index="/trend">趋势分析</el-menu-item>
            </el-submenu>
          </el-menu>
        </Sider>
        <Layout :style="{padding: '0 24px 24px'}" v-loading="loading">
          <div class="box">
            <el-select
                v-model="reportType"
                @change="changeReportType"
                placeholder="请选择"
                style="margin: 5px"
            >
              <el-option label="月度报告" value="month"> </el-option>
              <el-option label="年度报告" value="year"> </el-option>
            </el-select>
            <el-date-picker
                v-model="date"
                v-if="reportType==='month'"
                type="month"
                placeholder="选择月份"
                @change="changeDate"
            />
            <el-date-picker
                      v-model="date"
                      v-else
                      type="year"
                      placeholder="选择年份"
                      @change="changeDate"
            />
          </div>
          <router-view />
        </Layout>
      </Layout>
    </Layout>
  </div>
</template>
<script>
import { mapState, mapMutations } from "vuex";
export default {
  name: "layout",
  components: {},
  data() {
    return {
      date: new Date().setFullYear(2021, 11),
      reportType: "month",
    };
  },
  computed: {
    ...mapState(["date", "reportType", "loading"]),
  },
  methods: {
    changeReportType(reportType) {
      this.setReportType({ reportType });
    },
    changeDate(date) {
      this.setDate({ date });
    },
    ...mapMutations(["setDate", "setReportType"]),
  },
};
</script>
