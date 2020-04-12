<template>
  <div class="page-div">
    <div class="main-div"  v-for="(item,index) in productsCopy" :key="index">
      <div class="left-div">
        <img :src=item.ImageURL>
        <div class="offer">{{item.OfferText}}</div>
      </div>
      <div class="right-div">
        <h3>{{item.Brandname}}</h3>
        <h5>{{item.Productname}}</h5>
        <h5>{{item.Quantity}}</h5>
        <div class="mrp">MRP {{item.MRP}}</div>
        <div class="price">Rs {{item.Price}}</div>
        <div class="cart-div">
          <div><button class="cart-btn" :class="{ selected: item.count!=0}" @click="cartOperation(item,index,'add')" >ADD CART</button></div>
          <div class="op-div">
            <button class="add" :class="{ selected: item.Quantity <= item.count}" @click="cartOperation(item,index,'add')">+</button>
            <div class="count">{{item.count}}</div>
            <button class="add" :class="{ selected: item.count == 0}" @click="cartOperation(item,index,'sub')">-</button>
          </div>
        </div>
      </div>
    </div>
    <Footer :dataFromDisplay="qtyAndTotal" @dataFromFooter="checkOut"/>

    
    <!-- Modal -->
    <div :class="{ modaloverlay: chkout == true}">
      <div class="modal" v-show="isShowModal==1">
        <h3>Total Price : {{total}}</h3>
        <p>
          Transaction successful
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import Footer from './Footer.vue'

export default {
  name: 'DisplayList',
  components: {
    Footer
  },
  props: {},
  data() {
    return {
      productDetails:[],
      productsCopy:[],
      qty:0,
      total:0,
      qtyAndTotal:{},
      isShowModal:0,
      chkout:false,
    }
  },
  methods:{
    cartOperation(item,index,val){
      this.qty=0;
      this.total=0;
      console.log(item,index)
      var addCount = (val=='add') ? 1 : -1 ;
      const currentIndex =this.productsCopy.findIndex((el,i) => i ==index);
      Vue.set(this.productsCopy, currentIndex, {
            Brandname:this.productsCopy[currentIndex].Brandname,
            Productname:this.productsCopy[currentIndex].Productname,
            Quantity:this.productsCopy[currentIndex].Quantity ,
            Price:this.productsCopy[currentIndex].Price,
            MRP:this.productsCopy[currentIndex].MRP,
            ImageURL:this.productsCopy[currentIndex].ImageURL,
            OfferText:this.productsCopy[currentIndex].OfferText,
            count:this.productsCopy[currentIndex].count+addCount
      });
      var  pricePerTtem=0, qtyPerItem=0 ;
      this.productsCopy.forEach(el =>{
          qtyPerItem =el.count;
          pricePerTtem=el.Price*el.count;
          this.qty += qtyPerItem;
          this.total += pricePerTtem;
      })
       
      this.qtyAndTotal={
         qty :this.qty,
         total : this.total
      }
    },

     checkOut (event) {this.isShowModal=event; this.chkout=true}

  },
  created() {
     fetch('./data/details.json')
        .then(response => response.json())
        .then(data =>{
          this.productDetails=data.data
          this.productsCopy=JSON.parse(JSON.stringify(this.productDetails))
          this.productsCopy.forEach((el,index) => {
            this.productsCopy[index]['count']=0;
          });

        });
        this.qtyAndTotal={
         qty :0,
         total : 0
        }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.left-div{
  width: 50%;
  
}
.right-div{
      text-align: left;
}
.left-div img{
  width: 80px;
    margin-bottom: 20px;
}
.main-div{
  border-bottom: 1px solid blue;
    display: flex;
    width: 100%;
    margin-bottom: 35px;
    padding-bottom: 10px;
}
.offer{
  font-size: 20px;
}
h3 {
    color: #076d00;
    text-transform: capitalize;
    margin-top: 0px;
    margin-bottom: 10px;
}
h5 {
    font-size: 16px;
    margin-top: 0px;
    margin-bottom: 6px;
    text-transform: capitalize;
    font-weight: 100;
}
.price{
  margin-bottom: 8px;
    margin-top: 8px;
    font-size: 18px;
    font-weight: 600;
}
.mrp {
    text-transform: uppercase;
    font-size: 16px;
}
.cart-div{
  display: flex;
}
.cart-btn{
      background-color: #3be43b;
    font-weight: 900;
    padding: 7px 26px;
    -webkit-appearance: none;
    border: 0;
    border-radius: 7px;
    margin-right: 35px;
}
.add{
  background-color: #3be43b;
    font-weight: 900;
    padding: 10px 13px;
    -webkit-appearance: none;
    border: 0;
    border-radius: 50%;
    font-size: 20px;
    line-height: 11px;
}
.count{
  padding: 7px 17px;
}
.op-div{
  display: flex;
}
.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
  background: azure;
  padding: 1rem;
  border-radius: 1rem;
  border: 1px solid gray;
}
.selected{
  pointer-events: none;
  opacity: 0.5;
}
@media only screen and (max-width: 767px) {
  .cart-btn{
    width: 80px;
    padding: 12px 1px;
    margin-right: 5px;
  }
  .add{
    padding: 7px 13px;
    font-size: 17px;
  }
  .right-div{
    width: 60%;
  }
  .count{
    padding: 7px 9px;
  }
  .left-div{
    width: 37%;
  }
}
.page-div{
  margin-bottom: 80px;
}
.modaloverlay{
  height: 100vh;
    width: 100%;
    position: fixed;
    top: 0;
    z-index: 2;
}
</style>
