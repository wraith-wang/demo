<template>
  <div class="hello">
    <p>title: {{goods.title}}</p>
    <p>单价: {{goods.price}}</p>
    <p>款号: {{goods.goodsno}}</p>
    <p>品牌: {{goods.brand}}</p>
    <div class="table">
      <ul>
        <li>颜色/尺码</li>
        <li>M</li>
        <li>L</li>
        <li>S</li>
        <li>小计</li>
      </ul>
      <ul v-for="(item, index) in data1" :key="index">
        <li>{{item.color}}</li>
        <li><InputNumber :precision="0" :min="0" :max="item.mMax" v-model.number="item.m"></InputNumber></li>
        <li><InputNumber :precision="0" :min="0" :max="item.lMax" v-model.number="item.l"></InputNumber></li>
        <li><InputNumber :precision="0" :min="0" :max="item.sMax" v-model.number="item.s"></InputNumber></li>
        <li>{{item.m + item.l + item.s}}</li>
      </ul>
      <ul>
        <li>总件数:<span>{{totleNum}}</span>，总金额:{{totleNum * 100}}元</li>
        <li><Button type="primary" :disabled="totleNum < 1">提交订单</Button></li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      goods: {},
      data1: []
    }
  },
  created: function () {
    this.getData()
  },
  computed: {
    totleNum: function () {
      let num = 0
      for (let i = 0; i < this.data1.length; i++) {
        num += this.data1[i].m + this.data1[i].l + this.data1[i].s
      }
      return num
    }
  },
  methods: {
    getData: function () {
      this.$http.get('/static/data.json')
        .then(data => {
          console.log(data.body)
          this.goods = data.body
          for (let i = 0; i < data.body.inventory.length; i++) {
            let obj = {
              color: data.body.inventory[i].color,
              m: data.body.inventory[i].size.m,
              l: data.body.inventory[i].size.l,
              s: data.body.inventory[i].size.s,
              mMax: data.body.inventory[i].size.m,
              lMax: data.body.inventory[i].size.l,
              sMax: data.body.inventory[i].size.s
            }
            this.data1.push(obj)
          }
        }, err => {
          console.log(err)
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.table {
  width: 800px;
  margin: 0 auto;
}

.table ul {
  display: flex;
  height: 40px;
  align-items: center;
  border: 1px solid #dddee1;
  margin-bottom: -1px;
  padding: 0 10px;
}

.table ul li {
  flex: 1;
}

.table .ivu-input {
  width: 80%;
}
</style>
