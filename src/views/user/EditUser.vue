<template>
  <div style="width: 80%">
    <div style="margin-bottom: 30px"> 编辑用户</div>
    <el-form ref="ruleForm" :inline="true" :model="form" :rules="rules" label-width="100px">
      <el-form-item label="姓名" prop="name">
        <el-input v-model="form.name" placeholder="请输入姓名"></el-input>
      </el-form-item>
      <el-form-item label="年龄" prop="age">
        <el-input v-model="form.age" placeholder="请输入年龄"></el-input>
      </el-form-item>
      <el-form-item label="性别" prop="sex">
        <el-input v-model="form.sex" placeholder="请输入性别"></el-input>
      </el-form-item>
      <el-form-item label="联系方式" prop="phone">
        <el-input v-model="form.phone" placeholder="请输入联系方式"></el-input>
      </el-form-item>
      <el-form-item label="地址" prop="address">
        <el-input v-model="form.address" placeholder="请输入地址"></el-input>
      </el-form-item>
    </el-form>

    <div style="text-align: center; margin-top: 30px">
      <el-button size="medium" type="primary" @click="update">提交</el-button>
    </div>
  </div>
</template>

<script>
import request from "@/utils/request";

export default {
  name: "EditUser",
  data() {
    return {
      form: {}
    }
  },
  created() {
    // 页面一加载 就执行此方法
    // 根据id获取用户信息
    const id = this.$route.query.id;
    request.get('/user/' + id).then(res => {
      this.form = res.data;
    })
  },
  methods: {
    // 修改用户
    update() {
      request.put('/user/update', this.form).then(res => {
        if (res.code === '200') {
          this.$notify.success('修改成功')
          this.$router.push('/user')
        } else {
          this.$notify.error(res.msg)
        }
      })
    }
  }
}
</script>

<style scoped>

</style>