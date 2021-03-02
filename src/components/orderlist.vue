<template>
<div>
  <div class="title">Order A Drink</div>
  <div class="main">
    <v-sheet class="order-form">
      <div class="order-message">I wanna drink...</div>
      <div class="order-input">
        <div class="inputs">
          <input type="text" placeholder="🥤"  v-model="input.drink" class="input-text">
        </div>
        <div class="inputs">
          <input type="number" placeholder="$$" v-model="input.price" class="input-number">
        </div>
        <div class="inputs">
          <input type="number" placeholder="數量" v-model="input.qty" class="input-number">
        </div>
        <div class="inputs">
          <textarea placeholder="冰量甜度"  v-model="input.note" class="input-note"></textarea>
        </div>
      </div>
      <v-btn :color="colors.primary" @click="addDrink" class="button">add</v-btn>
    </v-sheet>
    <div class="orders">
      <div class="section-title">Orders</div>
      <div v-for="(item, index) of orderlist" :key="index" class="items">
        <div class="detail">
          <div class="name">{{ item.drink }}</div>
          <div class="price">＄{{ item.price }}</div>
          <div class="amount">{{ item.qty }} 杯</div>
          <div class="buttons">
            <v-btn :color="colors.primary" @click="toggle(index)" class="button">edit</v-btn>
            <v-btn :color="colors.primary" @click="deleteDrink(index)" class="button">delete</v-btn>
          </div>
        </div>
        <div class="note">{{ item.note }}</div>
      </div>
      <div v-if="showEdit" class="items edit-box">
        <div class="detail">
          <input type="text" v-model="edit.drink" class="edit-name">
          <input type="text" v-model="edit.price" class="edit-price">
          <input type="text" v-model="edit.qty" class="edit-qty">
        </div>
        <textarea v-model="edit.note" class="edit-note"></textarea>
        <button @click="editDrink" class="button edit-button">confirm</button>
      </div>
      <div class="items total-box">
        <div>💵 Total $ {{ totalPrice }} , {{ totalDrinks }} drinks</div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      orderlist: [
        {
          drink: '四季春一號',
          price: 40,
          note: '中杯 半糖去冰',
          qty: 1
        }
      ],
      input: {
        drink: '',
        price: null,
        note: '',
        qty: null
      },
      edit: {
        drink: '',
        price: null,
        note: '',
        qty: null
      },
      empty: false,
      showEdit: false,
      number: null,
      colors: {
        primary: 'rgb(232, 222, 181)'
      }
    }
  },
  computed: {
    totalDrinks() {
      let total = 0
      for(let i = 0; i < this.orderlist.length; i++) {
        total += parseInt(this.orderlist[i].qty)
      }
      return total
    },
    totalPrice() {
      let sum = 0
      for(let i = 0; i < this.orderlist.length; i++) {
        let subsum = parseInt(this.orderlist[i].price)*parseInt(this.orderlist[i].qty)
        sum += subsum
      }
      return sum
    }
  },
  methods: {
    addDrink() {
      this.orderlist.push(this.input)
      console.log(this.orderlist)
      this.input = {}
    },
    deleteDrink(index) {
      this.orderlist.splice(index, 1)
    },
    toggleEdit(index) {
      console.log(this.orderlist[index])
      this.orderlist[index].edit = true
    },
    toggle(index) {
      this.showEdit = !this.showEdit
      this.number = index
    },
    editDrink() {     
      this.orderlist.splice(this.number, 1, this.edit)
      this.showEdit = !this.showEdit
      this.edit = {}
    }
  }
}
</script>

<style scoped>
.title {
  font-size: 3rem;
  padding: 2rem 5rem;
}
.main {
  display: flex;
  flex-direction: row;
  padding: 1rem 5rem;
}
.order-form {
  width: 250px;
  padding: 2rem 1rem;
}
.order-message {
  margin-bottom: .8rem;
}
.order-input {
  display: flex;
  flex-direction: column;
  margin: .5rem 0;
}
.inputs {
  margin-bottom: .5rem;
}
.input-text {
  width: 80%;
  height: 1.5rem;
  border: 1px solid rgb(235, 229, 204);
  background-color: transparent;
}
.input-number {
  width: 80%;
  height: 1.5rem;
  border: 1px solid rgb(235, 229, 204);
  background-color: transparent;
}
.input-note {
  width: 80%;
  max-width: 80%;
  height: 5rem;
  max-height: 6rem;
  border: 1px solid rgb(235, 229, 204);
  background-color: transparent;

}
.orders {
  width: 500px;
  padding: 2rem 1rem;
}
.section-title {
  display: flex;
  justify-content: center;
}
.items {
  display: flex;
  flex-direction: column;
  background-color:rgba(235, 229, 204, .5);
  padding: 1rem .8rem;
  margin: .8rem 0;
  border-radius: 2px;
  box-shadow: 0 0 5px rgba(155, 155, 155, .3)
}
.detail {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  margin-bottom: .8rem;
  position: relative;
}
.name {
  margin-right: .8rem;
}
.price {
  margin-right: .8rem;
}
.buttons {
  position: absolute;
  right: 0;
}
.note {
  display: flex;
  justify-content: flex-start;
  border-top: 1px solid rgb(235, 229, 204);
  padding-top: .5rem;
}
.edit-name {
  border: 1px solid rgb(235, 229, 204);
  background-color: rgba(255, 255, 255, .3);
  height: 1.5rem;
  margin-right: .8rem;
}
.edit-price {
  border: 1px solid rgb(235, 229, 204);
  background-color: rgba(255, 255, 255, .3);
  height: 1.5rem;
  margin-right: .8rem;
}
.edit-qty {
  border: 1px solid rgb(235, 229, 204);
  background-color: rgba(255, 255, 255, .3);
  height: 1.5rem;
}
.edit-note {
  border: 1px solid rgb(235, 229, 204);
  background-color: rgba(255, 255, 255, .3);
  height: 1.5rem;
  max-width: 100%;
  height: 5rem;
  margin-bottom: 1rem;
}
.edit-button {
  width: 15%;
}
.button {
  border: none;
  background-color: rgb(232, 222, 181);
  border-radius: 2px;
  height: 1.5rem;
  margin-right: .5rem;
  color: rgb(99, 90, 52);
}
.items.total-box {
  box-shadow: 0 0 5px rgba(110, 106, 83, .3);
}
</style>
