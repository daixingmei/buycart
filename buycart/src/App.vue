<template>
  <div class="container">
    <table class="table">
      <tr class="row">
        <th class="col-2">全选
          <input type="checkbox" v-model="checkAll">
        </th>
        <th class="col-2">单价</th>
        <th class="col-3">数量</th>
        <th class="col-3">小计</th>
        <th class="col-2">操作</th>
      </tr>
      <tr v-for="(tableData,index) in tableData" class="row">
        <td class="col-2">
          <input type="checkbox" v-model="tableData.isSelect">
        </td>
        <td class="col-2">{{tableData.price}}</td>
        <td class="col-3">
          <input type="number" v-model.number="tableData.num" :min="0">
        </td>
        <td class="col-3">{{tableData.price*tableData.num|toFixed(2)}}</td>
        <td class="col-2">
          <button @click="remove(index,tableData)" class="btn btn-danger">删除</button>
        </td>
      </tr>
      <tr>
        <td>
          总价格：{{sum|toFixed(2)}}
        </td>
      </tr>
    </table>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tableData: [{
        num: 1,
        price: 51,
        count: 2,
        isSelect: false
      }, {
        num: 2,
        price: 52,
        count: 2,
        isSelect: false
      }, {
        num: 3,
        price: 53,
        count: 2,
        isSelect: false
      }],
    }
  },
  filters: {
    toFixed(input, param) {
      return '$' + input.toFixed(param)
    }
  },
  computed: {
    checkAll: {
      // 当数据变化时会重新计算（取值）
      get() {
        return this.tableData.every(item => item.isSelect)
      },
      // val给checkbox赋值时
      set(val) {
        this.tableData.forEach(item => item.isSelect = val)
      },
    },
    sum: {
      get() {
        return this.tableData.reduce((prev, next) => {
          if (!next.isSelect) return prev
          return prev + next.price * next.num
        }, 0)
      }
    }
  },
  methods: {
    remove(index, row) {
      console.log(index)
      this.$confirm('确定删除该商品？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        //删除数组中指定的元素
        // this.tableData.splice(index, 1);
        this.tableData = this.tableData.filter(item => item != row)
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    },
    // change(){
    //   this.tableData.forEach(item=>item.isSelect=this.checkAll)
    // },
    // checkone(){
    //   this.checkAll=this.tableData.every(item=>item.isSelect)
    // },

  }
}

</script>
