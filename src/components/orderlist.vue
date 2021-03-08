<template>
<div>
  <div class="title">Order A Drink</div>
  <div class="main">
    <v-sheet class="order-form">
      <div class="order-message">I wanna drink...</div>
      <v-form>
          <v-text-field type="text" label="🥤"  v-model="input.drink" :color="colors.inputs" hide-details outlined dense></v-text-field>
          <v-col class="d-flex justify-space-between px-0">
          <v-text-field type="number" label="$$" v-model="input.price" class="mr-1" :color="colors.inputs" hide-details outlined dense></v-text-field>
          <v-text-field type="number" label="數量" v-model="input.qty" :color="colors.inputs" hide-details outlined dense></v-text-field>
          </v-col>
          <v-textarea label="冰量甜度"  v-model="input.note" :color="colors.inputs" outlined dense></v-textarea>
      </v-form>
      <v-btn :color="colors.primary" @click="addDrink" class="button">add</v-btn>
    </v-sheet>
    <div class="orders">
      <div class="section-title">Orders</div>
      <v-card v-for="(item, index) of orderlist" :key="index" :color="colors.card" flat class="items">
        <div class="detail">
          <div class="detail-item">
            <div v-if="!item.edit" class="mr-3">{{ item.drink }}</div>
            <v-text-field v-if="item.edit" type="text" v-model="item.drink" hide-details :background-color="colors.inputs" outlined dense class="name mt-0 mr-3 pa-0" full-width></v-text-field>
            <div>$</div>
            <div v-if="!item.edit" class="mr-3">{{ item.price }}</div>
            <v-text-field v-if="item.edit" type="number" v-model="item.price" hide-details :background-color="colors.inputs" outlined dense class="price pa-0 mr-3"></v-text-field>
            <div v-if="!item.edit">{{ item.qty }}</div>
            <v-text-field v-if="item.edit" type="number" v-model="item.qty" hide-details :background-color="colors.inputs" outlined dense class="amount pa-0 mr-1"></v-text-field>
            <div>杯</div>
          </div>
          <div class="buttons">
            <v-icon v-if="!item.edit" @click="toggleEdit(index)" class="mr-3">mdi-pencil</v-icon>
            <v-icon v-else @click="saveEdit(index)" class="mr-3">mdi-check</v-icon>
            <v-icon @click="deleteDrink(index)">mdi-close</v-icon>
          </div>
        </div>
        <v-divider class="my-3"></v-divider>
        <div v-if="!item.edit" class="note">{{ item.note }}</div>
        <v-textarea v-else v-model="item.note" :background-color="colors.inputs" height="80" outlined dense filled></v-textarea>
      </v-card>
      <v-card :color="colors.card" flat class="items total-box">
        <div>💵 Total $ {{ totalPrice }} , {{ totalDrinks }} drinks</div>
      </v-card>
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
          qty: 1,
          edit: false
        }
      ],
      input: {
        drink: '',
        price: null,
        note: '',
        qty: null,
        edit: false
      },
      colors: {
        primary: 'rgb(232, 222, 181)',
        inputs: 'rgb(235, 229, 204)',
        card: 'rgba(235, 229, 204, .5)'
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
      this.input = {
        drink: '',
        price: null,
        note: '',
        qty: null,
        edit: false
      }
    },
    deleteDrink(index) {
      this.orderlist.splice(index, 1)
    },
    toggleEdit(index) {
      console.log(this.orderlist[index])
      this.orderlist[index].edit = true
    },
    saveEdit(index) {
      this.orderlist[index].edit = false
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
  align-items: center;
  margin-bottom: .8rem;
  position: relative;
}
.detail-item {
  display: flex;
  align-items: center;
  width: 82%;
}
.name {
  width: 45%;
}
.price {
  width: 15%;
}
.amount {
  width: 15%;
}
.buttons {
  position: absolute;
  right: 0;
}
.note {
  display: flex;
  justify-content: flex-start;
  /* border-top: 1px solid rgb(235, 229, 204); */
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
