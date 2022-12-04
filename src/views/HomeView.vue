<template>
  <div>
    <!--搜索表单-->
    <!---->
    <div style="margin-bottom: 20px">
      <el-input placeholder="请输入名称" style="width: 240px;"></el-input>
      <el-input placeholder="请输入联系方式" style="width: 240px;margin-left: 5px"></el-input>
      <el-button style="margin-left: 5px" type="primary"><i class="el-icon-search"></i>搜索</el-button>
    </div>
    <!--表格-->
    <el-table :data="tableData" stripe>
      <el-table-column label="名称" prop="name"></el-table-column>
      <el-table-column label="年龄" prop="age"></el-table-column>
      <el-table-column label="地址" prop="address"></el-table-column>
      <el-table-column label="联系方式" prop="phone"></el-table-column>
      <el-table-column label="性别" prop="sex"></el-table-column>
    </el-table>
    <!--分页-->
    <div style="margin-top: 20px">
      <el-pagination
          :total="100"
          background
          layout="prev, pager, next">
      </el-pagination>
    </div>
  </div>
</template>

<script>

export default {
  name: 'HomeView',
  data() {
    return {
      tableData: []
    }
  },
  created() {
    this.load()
  },
  methods: {
    load() {
      fetch('http://localhost:9090/user/list').then(res => res.json()).then(res => {
        console.log(res)
        this.tableData = res
      })
    }
  }
}
</script>
