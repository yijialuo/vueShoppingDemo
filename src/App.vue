<template>
  <div id="app">
    <template v-if="list.length">
      <table>
        <thead>
        <tr>
          <th>
            <button @click="allSelect">全选</button>
          </th>
          <!--<th></th>-->
          <th>商品名称</th>
          <th>商品单价</th>
          <th>购买数量</th>
          <th>操作</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(item,index) in list">
          <!--<td>{{index+1}}</td>-->
          <td><input type="checkbox" v-model="select[index]"/></td>
          <td>{{item.name}}</td>
          <td>{{item.price}}</td>
          <td>
            <button
              @click="handleReduce(index)"
              :disabled="item.count===1">-
            </button>
            {{item.count}}
            <button @click="handleAdd(index)">+</button>
          </td>
          <td>
            <button @click="handleRemove(index)">移除</button>
          </td>
        </tr>
        </tbody>
      </table>
      <div>总价：¥{{totalPrice}}</div>
    </template>
    <div v-else>
      购物车为空
    </div>
  </div>
</template>

<script>


  export default {
    name: 'App',
    data() {
      return {
        select: [],
        list: [{
          id: 1,
          name: 'iPhone 7',
          price: 6188,
          count: 1
        }, {
          id: 2,
          name: 'iPad Pro',
          price: 5888,
          count: 1
        }, {
          id: 3,
          name: 'MacBook Pro',
          price: 21488,
          count: 1
        }
        ]
      }
    },
    mounted() {
      for (let i = 0; i < this.list.length; i++) {
        this.select.push(false)
      }
    },
    computed: {
      totalPrice() {
        let total = 0;
        for (let i = 0; i < this.list.length; i++) {
          if (this.select[i]) {
            total += this.list[i].price * this.list[i].count
          }
        }
        //千位数 点
        return total.toString().replace(/\B(?=(\d{3})+$)/g, ',');
      }
    },
    methods: {
      allSelect() {
        for (let i = 0; i < this.select.length; i++) {
          if (this.select[i] != true)
            // this.select[i]=true 不行无法动态更新视图，需要splice
            this.select.splice(i,1,true)
        }
      },
      handleReduce(index) {
        if (this.list[index].count === 1) return;
        this.list[index].count--
      },
      handleAdd(index) {
        this.list[index].count++
      },
      handleRemove(index) {
        this.list.splice(index, 1)
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  table {
    border: 1px solid #e9e9e9;
    border-collapse: collapse;
    border-spacing: 0;
    empty-cells: show;
  }

  th, td {
    padding: 8px 16px;
    border: 1px solid #e9e9e9;
    text-align: left;
  }

  th {
    background: #f7f7f7;
    color: #5c6b77;
    font-weight: 600;
    white-space: nowrap;
  }
</style>
