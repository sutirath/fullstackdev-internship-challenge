<template >
  <div class="bg text-center" style=" padding-top:5rem">
    <mdb-container>
      <mdb-row>
        <mdb-col style="margin-right: 1rem">
          <mdb-row>
            <mdb-col
              sm="2"
              md="3"
              lg="4"
              v-for="item in items"
              :key="item.index"
              :class="className(item.id)"
            >
              <mdb-card waves style="margin-bottom: 1rem;" class="poiter">
                <mdb-view hover>
                  <mdb-card-image :src="item.image" alt="image" class="img" />
                  <mdb-mask flex-center waves overlay="white-slight"></mdb-mask>
                </mdb-view>
                <mdb-card-body>
                  <mdb-card-title>{{item.name}}</mdb-card-title>
                  <h5 class="text-primary">
                    <i class="fas fa-tags"></i>
                    {{item.price}} บาท
                  </h5>
                  <b-button
                    variant="primary"
                    @click="choose(item)"
                    :disabled="!item.in_stock"
                  >Select</b-button>
                </mdb-card-body>
              </mdb-card>
            </mdb-col>
          </mdb-row>
        </mdb-col>
        <mdb-col class="choosen shadow p-3 mb-5 bg-white rounded">
          <h1 class="text-left">Money {{money}} Bath</h1>
          <div>
            <mdb-row>
              <mdb-col sm="2" md="3" lg="3" v-for="coin in coins" :key="coin.id">
                <img
                  :src="coin.imgage"
                  alt="coins"
                  class="coins-style poiter  "
                  @click="AddMoney(coin.value)"
                  
                />
              </mdb-col>
            </mdb-row>
          </div>
          <br />
          <h1 class="text-left text-danger">Product : {{chooseItem.name}}</h1>
          <h1 class="text-left text-danger">Price : {{chooseItem.price}} ฿</h1>
          <b-button variant="primary" v-if="money>=chooseItem.price"  @click="Buy()">Buy</b-button>
          <b-button variant="danger" v-if="money>0"  @click="ChangeMoney()">ChangeMoney</b-button>
          <br />

          <b-modal
            v-model="Got_Item"
            centered
            ok-only
            @ok="ChangeMoney()"
            id="modal-center"
            title="You got item !!"
          >
            <center>
              <img v-if="Got_Item" :src="chooseItem.image" alt="gotItem" />
            </center>
          </b-modal>

          <div v-if="show">
            <h1 style="margin-top:2rem" class="text-success">Change Money {{changeMoney}} ฿</h1>
            <mdb-row>
              <mdb-col>
                <img src="@/assets/coins/1.png" alt="coins" class="coin-change" />
                = {{parseInt(this.coin_1)}}
              </mdb-col>
              <mdb-col>
                <img src="@/assets/coins/2.png" alt="coins" class="coin-change" />
                ={{parseInt(this.coin_2)}}
              </mdb-col>
              <mdb-col>
                <img src="@/assets/coins/5.png" alt="coins" class="coin-change" />
                = {{parseInt(this.coin_5)}}
              </mdb-col>
              <mdb-col>
                <img src="@/assets/coins/10.png" alt="coins" class="coin-change" />
                = {{parseInt(this.coin_10)}}
              </mdb-col>
            </mdb-row>
          </div>
        </mdb-col>
      </mdb-row>
      <br />
      <br />
    </mdb-container>
  </div>
</template>

<script>
import Axios from "axios";
import {
  mdbContainer,
  mdbRow,
  mdbCol,
  mdbCard,
  mdbCardImage,
  mdbCardBody,
  mdbCardTitle,
  mdbCardText,
  mdbBtn,
  mdbView,
  mdbMask
} from "mdbvue";
export default {
  data() {
    return {
      items: null,
      chooseItem: [],
      coins: [
        {
          id: 1,
          name: "1 Bath",
          value: 1,
          imgage:
            "https://firebasestorage.googleapis.com/v0/b/vending-machine-84489.appspot.com/o/1.png?alt=media&token=fa55931c-01cb-4ef8-b1c7-a43bde635843"
        },
        {
          id: 2,
          name: "2 Bath",
          value: 2,
          imgage:
            "https://firebasestorage.googleapis.com/v0/b/vending-machine-84489.appspot.com/o/2.png?alt=media&token=1fc632d2-39ce-488f-a999-b908bcb4783b"
        },
        {
          id: 3,
          name: "5 Bath",
          value: 5,
          imgage:
            "https://firebasestorage.googleapis.com/v0/b/vending-machine-84489.appspot.com/o/5.png?alt=media&token=4e38e9a3-8622-4d23-82a1-e51c6026ed84"
        },
        {
          id: 4,
          name: "10 Bath",
          value: 10,
          imgage:
            "https://firebasestorage.googleapis.com/v0/b/vending-machine-84489.appspot.com/o/10.png?alt=media&token=6489f98d-d6a5-4c1d-9804-6014a9c08a28"
        }
      ],
      money: 0,
      coin_10: 0,
      remain: 0,
      coin_5: 0,
      coin_2: 0,
      coin_1: 0,
      Got_Item: false,
      changeMoney: 0,
      show: false
    };
  },
  mounted() {
    Axios.get(`https://www.mocky.io/v2/5c77c5b330000051009d64c9`)
      .then(response => {
        this.items = response.data.data;
        //  console.log(this.items);
      })
      .catch(err => {
        console.log(err);
      });
  },
  methods: {
    choose(itemId) {
      if (this.chooseItem.id !== itemId.id) {
        this.chooseItem = [];
        this.chooseItem = itemId;
        this.changeMoney = 0;
        this.show = false;
        
      } else {
        this.show = false;
        this.changeMoney = 0;
        this.chooseItem = [];
      }
    },
    className(itemId) {
      return [{ choosen: itemId === this.chooseItem.id }];
    },
    AddMoney(coin) {
      this.money += coin;
      this.show = false;
    },
    ChangeMoney() {
      this.coin_10= 0
      this.remain= 0
      this.coin_5= 0
      this.coin_2= 0
      this.coin_1= 0
      if (this.money !== 0) {
        this.changeMoney = this.money;
        this.coin_10 = this.money / 10; // คำนวณจำนวนเหรียญ 10 บาท
        this.remain = this.money % 10; // จำนวนเงินที่เหลือจากการแลกเหรียญ 10 บาท
        this.coin_5 = this.remain / 5; // คำนวณจำนวนเหรียญ 5 บาท
        this.remain = this.remain % 5; // จำนวนเงินที่เหลือจากการแลกเหรียญ 5 บาท
        this.coin_2 = this.remain / 2; // คำนวณจำนวนเหรียญ 2 บาท
        this.remain = this.remain % 2; // จำนวนเงินที่เหลือจากการแลกเหรียญ 2 บาท
        this.coin_1 = this.remain; // จำนวนเงินที่เหลือจะเป็นเหรียญ 1
        this.money = 0;
        this.chooseItem = [];
        this.show = true;
      } else {
        this.chooseItem = [];
      }
    },
    Buy() {
      this.coin_10= 0
      this.remain= 0
      this.coin_5= 0
      this.coin_2= 0
      this.coin_1= 0
      if (this.chooseItem.length != 0) {
        if (this.money >= this.chooseItem.price) {
          if (this.chooseItem.in_stock == true) {
            this.money -= this.chooseItem.price;
            this.Got_Item = true;
          } else {
            alert("Out of stock");
            this.ChangeMoney();
          }
        } else {
          alert("Your money is not enough");
        }
      } else {
        alert("Not Select");
        
      }
    }
  },
  components: {
    mdbContainer,
    mdbRow,
    mdbCol,
    mdbCard,
    mdbCardImage,
    mdbCardBody,
    mdbCardTitle,
    mdbCardText,
    mdbBtn,
    mdbView,
    mdbMask
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Russo+One&display=swap");
.img {
  width: 160px;
  height: 120px;
}
.poiter {
  cursor: pointer;
}
.choosen {
  border-style: solid;
}
h1 {
  font-family: "Russo One", sans-serif;
}
.coins-style {
  width: 70px;
  height: 70px;
}
.coin-change {
  width: 60px;
  height: 60px;
}
.bg {
  background: rgb(56, 96, 224);
  background: linear-gradient(
    86deg,
    rgba(56, 96, 224, 1) 0%,
    rgba(15, 222, 255, 1) 70%,
    rgba(0, 151, 247, 1) 100%
  );
}

</style>