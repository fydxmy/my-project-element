<!-- eslint-disable no-unused-vars -->
<template>
  <div id="app">
    <div class="custom-add-form">
      <el-form
        :model="form"
        label-width="80px"
        label-position="left"
        size="large"
        :rules="rules"
        ref="hotForm"
        v-loading.fullscreen.lock="waiting"
      >
        <div
          class="hot-item"
          v-for="(hot, index) in form.hots"
          :key="index"
          :data-index="index + 1"
        >
          <el-form-item
            label="qqq"
            :prop="'hots.' + index + '.qqq'"
            :rules="rules.text"
          >
            <el-input
              v-model="hot.qqq"
              placeholder="请输入您要设置的热搜词汇"
            />
          </el-form-item>
          <el-form-item
            label="www"
            :prop="'hots.' + index + '.www'"
            :rules="rules.text"
          >
            <el-input v-model="hot.www" placeholder="请输入热搜类型" />
          </el-form-item>
          <el-form-item label="操作">
            <el-button type="warning" @click="insertItem(index)">
              新增一条
            </el-button>
            <el-button type="danger" @click="deleteItem(index)">
              删除此条
            </el-button>
          </el-form-item>
          <div
            class="hot-item"
            v-for="(hot, index) in hot.children"
            :key="index"
            :data-index="index + 1"
          >
            <el-form-item
              label="qqq"
              :prop="'hots.' + index + '.qqq'"
              :rules="rules.text1"
            >
              <el-input
                v-model="hot.qqq"
                placeholder="请输入您要设置的热搜词汇"
              />
            </el-form-item>
            <el-form-item
              label="www"
              :prop="'hots.' + index + '.www'"
              :rules="rules.text1"
            >
              <el-input v-model="hot.www" placeholder="请输入热搜类型" />
            </el-form-item>
          </div>
        </div>
      </el-form>
      <div>
        <el-button type="warning" @click="addItemHandler()">
          新增一条
        </el-button>
        <el-button type="primary" @click="submitForm()"> 表单提交 </el-button>
      </div>
    </div>
  </div>
</template>

<script>
// 自定义验证类型

export default {
  data() {
    return {
      // ：model必须是一个对象 虽然仅仅是一个数组 但是也要包装成对象
      form: {
        hots: [
          {
            qqq: "",
            www: "",
            children: [],
          },
        ],
      },
      rules: {
        text: [
          { required: true, message: "热搜词汇不能为空！", trigger: "blur" },
        ],
        text1: [
          { required: true, message: "热搜词汇不能为空！", trigger: "blur" },
          { validator: this.checkStype, trigger: "blur" },
        ],
        stype: [{ validator: this.checkStype, trigger: "blur" }],
      },
      waiting: false,
    };
  },
  created() {},
  updated() {
    console.log(this.data);
  },
  methods: {
    submitForm() {
      this.$refs.hotForm.validate(async (valid) => {
        // 如果能通过验证
        if (valid) {
          this.waiting = true;
          const hotsStr = JSON.stringify(this.form.hots);
          if (hotsStr.success) {
            setTimeout(() => {
              this.waiting = false;
              window.alert("热搜保存成功！");
            }, 1000);
          } else {
            this.waiting = false;
            window.alert("热搜保存失败");
          }
        }
        // 当不能通过验证的时候
        else {
          window.alert("您的表单验证有误，请根据提示进行修改");
          return;
        }
      });
    },
    deleteItem(index) {
      this.data[index].children.push({
        qqq: "",
        www: "",
      });
      console.log(
        "%c [ index ]-115",
        "font-size:13px; background:pink; color:#bf2c9f;",
        index
      );
    },
    addItemHandler() {
      this.form.hots.push({
        qqq: "",
        www: "",
        children: [],
      });
    },
    // 3级表单效验
    checkStype(rule, value, callback) {
      console.log(rule, value, callback);
      const itemPath = rule.field.split('.');
      if(this.form.hots?.[itemPath[1]]?.children.filter(item => item[itemPath[2]] === value).length === 2 ) {
        return callback(new Error("当前key重复"));
      }
      callback();
    },
    insertItem(index) {
      this.form.hots[index].children.push({
        qqq: "",
        www: "",
      });
      console.log(
        "%c [ index ]-115",
        "font-size:13px; background:pink; color:#bf2c9f;"
      );
      console.log(
        "%c [ index ]-120",
        "font-size:13px; background:pink; color:#bf2c9f;",
        index
      );
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
}
.custom-add-form {
  width: 900px;
}
.hot-item {
  display: flex;
  flex-wrap: wrap;
}
</style>
