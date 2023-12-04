<template>
    <div>
      <Breadcrumb :style="{margin: '24px 0'}">
        <BreadcrumbItem>案件检索</BreadcrumbItem>
        <BreadcrumbItem>类案检索</BreadcrumbItem>
      </Breadcrumb>
      <Content :style="{padding: '24px', minHeight: '280px', background: '#fff'}">
        <h1 class="title">类案检索</h1>
  
        <!-- 检索选项 -->
        <el-select v-model="searchType" placeholder="请选择检索类型" style="width: 200px;">
          <el-option label="处罚依据" value="basis"></el-option>
          <el-option label="罚金" value="fine"></el-option>
          <el-option label="当事人名称" value="partyName"></el-option>
          <el-option label="机构名称" value="organName"></el-option>
        </el-select>
  
        <el-input
          v-model="searchInput"
          placeholder="请输入检索内容"
          :style="{ width: '70%', marginLeft: '10px', marginRight: '10px' }"
        ></el-input>
        <el-button type="primary" @click="searchCases">检索</el-button>
  
        <el-divider></el-divider>
  
        <el-table :data="filteredCases" style="width: 100%">
          <el-table-column prop="name" label="案件名称"></el-table-column>
          <el-table-column prop="number" label="案件编号"></el-table-column>
          <el-table-column prop="type" label="案件类型"></el-table-column>
          <!-- 可以根据返回数据的内容添加更多列 -->
        </el-table>
      </Content>
    </div>
  </template>
  
  <script>
  import { mapActions, mapState } from 'vuex';
  // 引入API函数
  import { getSimilarCasesByBasis, getSimilarCasesByFine, getSimilarCasesByPartyName, getSimilarCasesByOrganName } from '@/api/api';
  
  export default {
    data() {
      return {
        searchType: '',
        searchInput: '',
        cases: [],
        filteredCases: []
      };
    },
    computed: {
      ...mapState(['cases'])
    },
    methods: {
      ...mapActions(['searchCases']),
      searchCases() {
        let params = { penaltyId: this.searchInput }; // 根据需要调整参数
        switch (this.searchType) {
          case 'basis':
            getSimilarCasesByBasis(params).then(response => this.cases = response.data);
            break;
          case 'fine':
            getSimilarCasesByFine(params).then(response => this.cases = response.data);
            break;
          case 'partyName':
            getSimilarCasesByPartyName(params).then(response => this.cases = response.data);
            break;
          case 'organName':
            getSimilarCasesByOrganName(params).then(response => this.cases = response.data);
            break;
        }
      },
    }
  };
  </script>
  

<style scoped>
.title {
  text-align: center;
}
</style>
