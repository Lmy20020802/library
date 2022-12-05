<template>
  <div>
    <!--搜索表单-->
    <!---->
    <div style="margin-bottom: 20px">
      <el-input v-model="params.name" placeholder="请输入名称" style="width: 240px;"></el-input>
      <el-input v-model="params.phone" placeholder="请输入联系方式" style="width: 240px;margin-left: 5px"></el-input>
      <el-button style="margin-left: 5px" type="primary" @click="load"><i class="el-icon-search"></i>搜索</el-button>
      <el-button style="margin-left: 5px" type="warning" @click="reset"><i class="el-icon-refresh"></i>重置</el-button>
    </div>
    <!--表格-->
    <el-table :data="tableData" stripe>
      <el-table-column label="编号" prop="id" width="80px"></el-table-column>
      <el-table-column label="名称" prop="name"></el-table-column>
      <el-table-column label="年龄" prop="age"></el-table-column>
      <el-table-column label="地址" prop="address"></el-table-column>
      <el-table-column label="联系方式" prop="phone"></el-table-column>
      <el-table-column label="性别" prop="sex"></el-table-column>
      <el-table-column label="创建时间" prop="createTime"></el-table-column>
      <el-table-column label="更新时间" prop="updateTime"></el-table-column>
      <el-table-column label="操作">
        <template v-slot="scope">
          <!--scope.row是当前行数据-->
          <el-button type="primary" @click="$router.push('editUser?id='+scope.row.id)">编辑</el-button>
          <el-popconfirm
              style="margin-left: 5px"
              title="您确定要删除这行数据吗？"
              @confirm="del(scope.row.id)"
          >
            <el-button slot="reference" type="danger">删除</el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    <!--分页-->
    <div style="margin-top: 20px">
      <el-pagination
          :current-page="params.pageNum"
          :page-size="params.pageSize"
          :total="total"
          background
          layout="prev, pager, next"
          @current-change="handleCurrentChange">
      </el-pagination>
    </div>
  </div>
</template>

<script>
import request from "@/utils/request";

export default {
  name: 'User',
  data() {
    return {
      tableData: [],
      total: 0,
      params: {
        pageNum: 1,
        pageSize: 5,
        name: '',
        phone: ''
      }
    }
  },
  created() {
    // 页面一刷新就执行
    this.load()
  },
  methods: {
    // 分页条件查询
    load() {
      // fetch('http://localhost:9090/user/list').then(res => res.json()).then(res => {
      //   console.log(res)
      //   this.tableData = res
      // })

      request.get('/user/page', {
        params: this.params
      }).then(res => {
        if (res.code === '200') {
          this.tableData = res.data.list
          this.total = res.data.total
        }
      })
    },

    // 页数跳转的具体方法
    handleCurrentChange(pageNum) {
      // 点击分页按钮触发分页
      this.params.pageNum = pageNum
      this.load()
    },

    // 重置按钮
    reset() {
      this.params = {
        pageNum: 1,
        pageSize: 2,
        name: '',
        phone: ''
      }
      this.load()
    },

    //删除按钮
    del(id) {
      request.delete('/user/delete/' + id).then(res => {
        if (res.code === '200') {
          this.$notify.success('删除成功')
          this.load()
        } else {
          this.$notify.error('删除失败')
        }
      })

    }
  }
}
</script>