<template>
  <div class="points-shop">
    <header>
      <h1>🎁 积分商城</h1>
      <p class="points">当前积分：<strong>{{ currentPoints }}</strong></p>
    </header>

    <section class="goods-list">
      <div
        v-for="(item, index) in goods"
        :key="index"
        class="goods-item"
      >
        <img :src="item.image" alt="商品图片" class="goods-image" />
        <div class="goods-info">
          <h2 class="goods-name">{{ item.name }}</h2>
          <p class="goods-price">
            需要积分：<span>{{ item.price }}</span>
          </p>
          <button
            @click="redeem(item)"
            :disabled="currentPoints < item.price"
          >
            立即兑换
          </button>
        </div>
      </div>
    </section>

    <!-- 如果未来需要分页加载，可以在此处加入"加载更多"按钮 -->
    <div class="load-more" v-if="hasMore">
      <button @click="loadMore">加载更多</button>
    </div>

    <!-- 兑换成功弹窗 -->
    <div v-if="modalVisible" class="modal-overlay" @click.self="closeModal">
      <div class="modal-content">
        <img :src="redeemedItem.image" alt="商品图片" class="modal-image" />
        <h2>兑换成功</h2>
        <p>{{ redeemedItem.name }} 已兑换成功！</p>
        <p>剩余积分：{{ currentPoints }}</p>
        <button @click="closeModal">关闭</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Shop',
  data() {
    return {
      currentPoints: 100,
      // 用示例数组表示商品列表，可以根据需要拆分分页加载
      goods: [
        { name: '蜜雪冰城甜筒', price: 100, image: require('@/assets/tiantong.jpg') },
        { name: '蜜雪冰城柠檬水', price: 100, image: require('@/assets/ningmeng.jpg') },
        { name: '显示器', price: 200, image: require('@/assets/显示器.jpg') },
        { name: '显卡', price: 120, image: require('@/assets/xianka.jpg') },
        { name: '哪吒', price: 120, image: require('@/assets/nezha.jpg') },
        { name: '科技树周边', price: 120, image: require('@/assets/科技树.png') },
        { name: '书包', price: 120, image: require('@/assets/shubao.jpg') },
        { name: '景区门票', price: 120, image: require('@/assets/menpiao.jpg') },
        { name: '篮球', price: 120, image: require('@/assets/lanqiu.jpg') },
        { name: '点卷', price: 120, image: require('@/assets/dianjuan.jpg') },
        { name: '肯德基全家桶', price: 120, image: require('@/assets/kedeji.jpg') },
        { name: '水弹枪', price: 120, image: require('@/assets/gan.jpg') },
        { name: '可乐', price: 120, image: require('@/assets/kele.jpg') },
        { name: '鼠标', price: 120, image: require('@/assets/shubiao.jpg') },
        { name: '冰箱贴', price: 120, image: require('@/assets/tiezhi.jpg') },
        { name: '网球拍', price: 120, image: require('@/assets/wangqiu.jpg') },
        { name: '无人机', price: 120, image: require('@/assets/wurenji.jpg') },
        { name: '100元现金', price: 120, image: require('@/assets/yuan.jpg') },
        { name: '羽毛球', price: 120, image: require('@/assets/yumaoq.jpg') },
        { name: '羽毛球拍', price: 120, image: require('@/assets/yumaoqiu.jpg') },
        { name: '纸牌', price: 120, image: require('@/assets/zhipai.jpg') },
        // … 如果商品很多，可以放到一个数组中

      ],
      modalVisible: false, // 弹窗是否显示
      redeemedItem: {}, // 兑换成功的商品
      hasMore: false // 默认一次性加载，如果需要分页可设置为 true
    };
  },
  methods: {
    redeem(item) {
      if (this.currentPoints >= item.price) {
        this.currentPoints -= item.price;
        this.redeemedItem = item;
        this.modalVisible = true;
        // 这里可以添加调用后端接口更新兑换记录
      } else {
        alert('积分不足，无法兑换此奖品');
      }
    },
    closeModal() {
      this.modalVisible = false;
    },
    // 示例：加载更多商品的方法（如果需要分页）
    loadMore() {
      // 这里可以通过 API 获取下一页的商品
      // 假设返回一个数组 newGoods，我们将其合并到当前 goods 中
      // 比如：this.goods = [...this.goods, ...newGoods];
      // 同时根据返回数据判断是否还存在更多商品，更新 this.hasMore
    }
  }
};
</script>

<style scoped>
/* 整体页面容器铺满整个视口 */
.points-shop {
  min-height: 100vh;   /* 保证最小高度为视口高度 */
  max-width: 3000px;
  margin: 0 auto;
  padding: 20px 10px;
  background: linear-gradient(135deg, #f5f7fa, #e2eafc);
  border-radius: 16px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

/* 头部样式 */
header {
  text-align: center;
  margin-bottom: 30px;
  padding: 0 10px;
}

header h1 {
  font-size: 2.5rem;
  margin-bottom: 10px;
  color: #333;
}

header .points {
  font-size: 1.2rem;
  color: #555;
}

/* 商品列表区域，使用 grid 布局自适应 */
.goods-list {
  flex-grow: 1;    /* 让商品区域填满剩余空间 */
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 20px;
}

/* 商品卡片样式 */
.goods-item {
  background-color: #fff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08);
  transition: transform 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.goods-item:hover {
  transform: translateY(-5px);
}

/* 商品图片样式 */
.goods-image {
  width: 100%;
  height: 180px;
  object-fit: contain;
  background-color: #f8f8f8;
  padding: 10px;
}

/* 商品信息区域 */
.goods-info {
  padding: 15px;
  text-align: center;
}

.goods-name {
  font-size: 1.2rem;
  margin: 10px 0 6px;
  color: #333;
}

.goods-price {
  color: #888;
  margin-bottom: 12px;
}

.goods-price span {
  color: #ff6600;
  font-weight: bold;
}

/* 按钮样式 */
button {
  background: linear-gradient(to right, #42b983, #2c8f6b);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 30px;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.3s ease;
}

button:hover:not(:disabled) {
  background: linear-gradient(to right, #2c8f6b, #1e6d52);
}

button:disabled {
  background: #ccc;
  cursor: not-allowed;
}

/* 加载更多按钮容器 */
.load-more {
  text-align: center;
  margin: 20px 0;
}

/* 弹窗遮罩 */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

/* 弹窗内容 */
.modal-content {
  background-color: #fff;
  padding: 30px;
  border-radius: 16px;
  text-align: center;
  max-width: 90%;
  width: 400px;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
  animation: fadeIn 0.3s ease;
}

.modal-image {
  width: 150px;
  height: 150px;
  object-fit: contain;
  margin-bottom: 20px;
}

/* 渐入动画效果 */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 响应式样式 */
@media (max-width: 768px) {
  header h1 {
    font-size: 2rem;
  }
  header .points {
    font-size: 1rem;
  }
  .goods-image {
    height: 150px;
  }
  .modal-content {
    width: 90%;
    padding: 20px;
  }
  button {
    padding: 8px 16px;
    font-size: 13px;
  }
}

@media (max-width: 480px) {
  .goods-list {
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 15px;
  }
  .goods-item {
    border-radius: 12px;
  }
  .goods-info {
    padding: 10px;
  }
  header h1 {
    font-size: 1.8rem;
  }
}
</style>
