<template>
  <div id="myAddress">
    <h1>我的收货地址</h1>
    <div v-for="(item,index) of itemList" :key="index">
      <table class="myAddressTable">
        <tr>
          <td>
            <label>姓名：</label>
          </td>
          <td class="myAddressShow">
            <span>{{item.receiverName}}</span>
          </td>
          <td>
            <button type="submit" class="myAddressButton" v-on:click="_revise(index)">编辑地址</button>
          </td>
        </tr>
        <tr>
          <td>
            <label>联系电话：</label>
          </td>
          <td class="myAddressShow">{{item.receiverPhone}}</td>
          <td>
            <label>
              <input type="radio" checked v-model="radioId" :value="item.receiverId" />
            </label>
          </td>
        </tr>
        <tr>
          <td>
            <label>收货地址：</label>
          </td>
          <td class="myAddressShow" colspan="2">{{item.receiverAddressInfo}}</td>
        </tr>
      </table>
      <br />
    </div>

    <br />

    <div>
      <button type="submit" class="myAddressButton" v-on:click="_commit">提交地址</button>
      <button type="submit" class="myAddressButton" v-on:click="_add">新增地址</button>
      <button type="submit" class="myAddressButton" v-on:click="_delete(radioId)">删除地址</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'address',

  data () {
    return {
      getUserName: '',
      radioId: '',
      itemList: []
    }
  },
  mounted () {
    this.getUserName = sessionStorage.getItem('getUserName')
    this.getData()
  },

  methods: {
    getData () {
      axios
        .get('/receiver/list/userName=' + this.getUserName)
        .then(response => {
          this.itemList = response.data
          console.log(typeof response.data)
        })
        .catch(function (error) {
          console.log(error)
        })
    },

    _revise: function (index) {
      window.location.href = '/address/editAddress?id=' + index
    },

    _commit: function () {
      // 待开发中。。。
      alert('开发中，敬请期待！')
    },

    _add: function () {
      window.location.href = '/address/newAdd'
    },

    _delete: function (radioId) {
      console.log('被选中的值为:' + radioId)
      axios
        .delete(
          '/receiver/delReceiver/userName=' +
            this.getUserName +
            '&receiverId=' +
            radioId
        )
        .then(function (response) {
          console.log(response)
          window.location.reload()
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}
</script>

<style lang="stylus" scoped>
#myAddress {
  position: absolute;
  width: 600px;
  left: 50%;
  margin-left: -300px;
  border: 1px solid #000;
  margin-top: 10px;
}

.myAddressTable {
  text-align: left;
}

.myAddressShow {
  width: 300px;
  text-decoration: underline;
}

.myAddressButton {
  margin-right: 100px;
}
</style>
