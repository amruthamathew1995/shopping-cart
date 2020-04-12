<template>
  <div class="footer">
    <div class="ftr-left">
      <div>qty {{dataFromDisplay.qty}}</div>
      <div>Total {{dataFromDisplay.total}}</div>
    </div>
    <div class="ftr-right">
      <button class="checkout" :class="{ selected: dataFromDisplay.qty==0}" @click="clickCheckout">checkout</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Footer',
  props: {
   dataFromDisplay:{
      type: Object
   }
  },
  methods:{
     clickCheckout () {this.$emit('dataFromFooter', 1)}
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.footer{
  background-color: #c166c0;
    position: fixed;
    bottom: 0;
    width: 100%;
    display: flex;
}
.ftr-left{
      width: 50%;
    font-size: 24px;
    font-weight: 700;
    color: #262577;
    text-transform: capitalize;
}
.checkout{
  margin-top: 8px;
    background-color: #3be43b;
    font-weight: 900;
    padding: 7px 26px;
    border: 0;
    border-radius: 7px;
    text-transform: uppercase;
    height: 41px;
}
.ftr-right{
  width: 50%;
}
.selected{
  pointer-events: none;
  opacity: 0.5;
}
@media only screen and (max-width: 767px) {
.footer{
  
    padding: 6px 0px;
  }
  .checkout{
    margin-top: 12px;
  }
  .ftr-left{
    text-align: left;
  }
}
</style>
