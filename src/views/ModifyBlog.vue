 <!--
 * 厦门大学计算机专业 | 前华为工程师
 * 专注《零基础学编程系列》  http://lblbc.cn/blog
 * 包含：Java | 安卓 | 前端 | Flutter | iOS | 小程序 | 鸿蒙
 * 公众号：蓝不蓝编程
 -->
<template>
  <div class="data-wrapper">
    <el-form ref="form" :model="form" :rules="rules" class="data-box">
      <el-form-item prop="content">
        <el-input type="textarea" placeholder="请输入正文" :rows="20" v-model="form.content"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="modifyNote()">保存</el-button>
      </el-form-item>
    </el-form>

    <el-dialog title="温馨提示" :visible.sync="dialogVisible" width="30%">
      <span>{{ msg }}</span>
      <span slot="footer" class="dialog-footer">
            <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
        </span>
    </el-dialog>
  </div>
</template>

<script>
import {modifyNote, queryNote} from "../api/note_api";

export default {
  name: "ModifyNote",
  data() {
    return {
      form: {
        content: ''
      },
      msg: '',
      // 表单验证，需要在 el-form-item 元素中增加 prop 属性
      rules: {
        content: [{
          required: true,
          message: '内容不可为空',
          trigger: 'blur'
        }]
      },

      // 对话框显示和隐藏
      dialogVisible: false
    }
  },
  methods: {
    async modifyNote() {
      modifyNote(this.$route.query.id, this.form).then(res => {
        this.$router.push("/note/list");
      }).catch(error => {
        alert('更新失败');
        console.log(error);
      });
    }
  },
  mounted() {
    queryNote(this.$route.query.id).then(res => {
      this.form.content = res.data.data.content
    }).catch(error => {
      console.log(error);
    });
  }
}
</script>

<style>
.data-wrapper {
  position: absolute;
  height: 100%;
  width: 100%;
}

.data-box {
  border: 1px solid #DCDFE6;
  width: 80%;
  margin: 0 auto;
  padding: 35px 35px 15px 35px;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
}
</style>
