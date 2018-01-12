<template >
  <div class="container" id="box">
    <form class="form">
      <div class="form-group">
        <label class="username">用户名</label>
        <input type="text" id='username' class='form-control' placeholder='请输入用户名' v-model='username' />
      </div>
      <div class="form-group">
        <label class="age">年龄</label>
        <input type="text" id='age' class='form-control' placeholder='请输入年龄' v-model='age' />
      </div>
      <div class="form-group">
        <input type="button" value='添加' class='btn btn-primary btn-sm' v-on:click="add()" />
        <input type="reset" value='重置' class='btn btn-danger btn-sm' @click='reset()' />
      </div>
    </form>
    <hr>
    <div>
      <p class="h3 text-info text-center">用户信息表</p>
      <table class="table table-bordered table-hover text-center">
        <tbody>
          <tr>
            <th>序号</th>
            <th>名字</th>
            <th>年龄</th>
            <th>操作</th>
          </tr>
          <tr v-for="(item,index) in myData ">
            <td>{{index+1}}</td>
            <td>{{item.name}}</td>
            <td>{{item.age}}</td>
            <td>
              <input type="button" class="btn btn-danger btn-sm" value='删除' data-toggle='modal' data-target='#layer' v-on:click="nowIndex = index" />
            </td>
          </tr>
          <tr v-if='myData.length!=0' class="text-right">
            <td colspan="4">
              <input type="button" class="btn btn-danger btn-sm" value='删除全部' @click='nowIndex = -2' data-toggle='modal' data-target='#layer' />
            </td>
          </tr>
          <tr v-if='myData.length==0'>
            <td colspan="4" class="text-center text-muted">
              <p>暂无数据 </p>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div role='dialog' class="modal fade bs-examle-modal-sm" id="layer">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss='modal'>
              <span>&times.</span>
            </button>
            <h4 class="modal-title">确认删除吗</h4>
          </div>
          <div class="modal-body text-right">
            <button data-dismiss='modal' type="button" class="btn btn-primary btn-sm">取消</button>
            <button data-dismiss='modal' type="button" class="btn btn-danger btn-sm" @click='remove(nowIndex)'>确认</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>   

<script>
const STORAGE_KEY = "todos-vuejs";
export default {
  name: "HelloWorld",
  data() {
    return {
      myData: JSON.parse(window.localStorage.getItem(STORAGE_KEY) || "[]"),
      username: "",
      age: "",
      nowIndex: -100
    };
  },
  methods: {
    //添加用户
    add: function() {
      this.myData.push({ name: this.username, age: this.age });
      this.username = "";
      this.age = "";
    },
    //删除用户
    remove: function(n) {
      if (n == -2) {
        this.myData = [];
      } else {
        this.myData.splice(n, 1);
      }
    },
    //重置输入框
    reset: function() {
      this.username = "";
      this.age = "";
    }
  },
  //监听数据变化并存入到localStorage中
  watch: {
    myData: {
      handler(items) {
        window.localStorage.setItem(STORAGE_KEY, JSON.stringify(items));
      },
      deep: true
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
th {
  text-align: center;
}
.form-group {
  text-align: left;
}
</style>