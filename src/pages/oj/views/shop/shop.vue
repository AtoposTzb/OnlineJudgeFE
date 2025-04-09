<template>
    <div class="points-shop">
      <header>
        <h1>积分商城</h1>
        <p>当前积分：{{ currentPoints }}</p>
      </header>
      <section class="goods-list">
        <div v-for="(item, index) in goods" :key="index" class="goods-item">
          <img :src="item.image" alt="商品图片" class="goods-image" />
          <div class="goods-info">
            <h2 class="goods-name">{{ item.name }}</h2>
            <p class="goods-price">需要积分：{{ item.price }}</p>
            <button @click="redeem(item)">立即兑换</button>
          </div>
        </div>
      </section>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Shop',
    data() {
      return {
        // 模拟用户当前积分，后续可结合 Vuex 或 API 实现实时更新
        currentPoints: 100,
        // 模拟商品数据，确保所有引用图片的文件在 src/assets 下存在
        goods: [
          { name: '乒乓球', price: 120, image: require('@/assets/1.png') },
          { name: '篮球', price: 80, image: require('@/assets/2.png') },
          { name: '文具', price: 50, image: require('@/assets/3.png') },
          { name: '球鞋', price: 120, image: require('@/assets/4.png') },
          { name: '足球', price: 120, image: require('@/assets/5.png') },
          { name: '书包', price: 120, image: require('@/assets/6.png') },
          { name: '水杯', price: 120, image: require('@/assets/7.png') },
          { name: '小熊公仔', price: 120, image: require('@/assets/8.png') }

        ]
      }
    },
    methods: {
      redeem(item) {
        if (this.currentPoints >= item.price) {
          this.currentPoints -= item.price;
          alert(`成功兑换 ${item.name}！剩余积分：${this.currentPoints}`);
          // 这里可以添加调用接口更新后端兑换记录的代码
        } else {
          alert('积分不足，无法兑换此奖品');
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .points-shop {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }
  header {
    text-align: center;
    margin-bottom: 20px;
  }
  .goods-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }
  .goods-item {
    border: 1px solid #eaeaea;
    border-radius: 4px;
    width: 200px;
    margin: 10px;
    text-align: center;
    padding: 10px;
    box-shadow: 2px 2px 5px rgba(0,0,0,0.1);
  }
  .goods-image {
    width: 100%;
    height: auto;
  }
  .goods-info {
    margin-top: 10px;
  }
  button {
    background-color: #42b983;
    color: #fff;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
  }
  button:hover {
    background-color: #2c8f6b;
  }
  </style>
  