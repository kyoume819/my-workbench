<script setup>
import { ref, computed } from 'vue'

// 当前页面：home / detail / cart / orders / member / favorites / live / flashcoupon
const currentPage = ref('home')
const currentProduct = ref(null)

// 商品分类
const categories = ['全部', '手机数码', '服饰鞋包', '美妆护肤', '食品生鲜', '家居家装']
const activeCategory = ref('全部')

// 商品数据
const products = ref([
  { id: 1, name: 'Apple iPhone 15 Pro 256G 原色钛金属 官方标配', price: 8999, originalPrice: 9999, sales: 23000, category: '手机数码', emoji: '📱', tags: ['天猫', '包邮', '7天无理由'], shop: 'Apple官方旗舰店', desc: 'A17 Pro芯片，钛金属设计，4800万像素三摄，支持USB-C接口，全天候显示。', detail: 'iPhone 15 Pro采用航空级钛金属打造，重量更轻、强度更高。搭载A17 Pro芯片，支持游戏级主机性能。4800万像素主摄，支持5倍光学变焦。USB-C接口，兼容更多配件。' },
  { id: 2, name: 'Nike Air Force 1 空军一号 经典白色 男女同款', price: 799, originalPrice: 899, sales: 45000, category: '服饰鞋包', emoji: '👟', tags: ['天猫', '包邮'], shop: 'Nike官方旗舰店', desc: '经典空军一号，皮革鞋面，舒适缓震，百搭款。', detail: 'Nike Air Force 1 诞生于1982年，是篮球鞋历史上的传奇之作。采用全粒面皮革鞋面，内置Air气垫，舒适耐穿。经典白色，男女同款，百搭各种穿搭风格。' },
  { id: 3, name: '兰蔻小黑瓶精华肌底液 50ml 全新升级版', price: 1080, originalPrice: 1380, sales: 12000, category: '美妆护肤', emoji: '🧴', tags: ['天猫', '正品保障'], shop: '兰蔻官方旗舰店', desc: '二裂酵母发酵产物，修护肌底，改善细纹。', detail: '全新升级小黑瓶，蕴含更多二裂酵母发酵产物溶胞物，深入肌底修护，改善肤质，让肌肤更细腻光滑。连续使用28天，肌肤状态明显提升。' },
  { id: 4, name: '三只松鼠坚果大礼包1500g 每日坚果混合装送礼礼盒', price: 89.9, originalPrice: 129.9, sales: 89000, category: '食品生鲜', emoji: '🥜', tags: ['包邮', '特价'], shop: '三只松鼠旗舰店', desc: '1500g超大分量，8种坚果混合，送礼佳品。', detail: '三只松鼠坚果大礼包，内含8种精选坚果：核桃、腰果、巴旦木、夏威夷果、开心果、榛子、花生、瓜子。1500g超大分量，独立小包装，方便携带。节日送礼首选。' },
  { id: 5, name: '戴森V12 Detect Slim 无线吸尘器 家用大吸力除螨', price: 3690, originalPrice: 4290, sales: 5600, category: '家居家装', emoji: '🏠', tags: ['天猫', '新品'], shop: 'Dyson戴森旗舰店', desc: '激光探测微尘，60分钟续航，吸力强劲。', detail: '戴森V12 Detect Slim，配备激光探测技术，让肉眼不可见的微尘现形。60分钟超长续航，多吸头配置，应对全屋清洁。重量轻，手持不累。' },
  { id: 6, name: '华为Mate 60 Pro 12+512G 雅川青 全网通5G智能手机', price: 6499, originalPrice: 6999, sales: 34000, category: '手机数码', emoji: '📱', tags: ['现货', '包邮'], shop: '华为官方旗舰店', desc: '麒麟9000S芯片，卫星通信，超光变影像。', detail: '华为Mate 60 Pro，搭载麒麟9000S芯片，支持双向北斗卫星消息。超光变XMAGE影像系统，5000mAh大电池，88W超级快充。' },
  { id: 7, name: '优衣库纯棉圆领T恤 男女同款 春夏打底衫 短袖上衣', price: 99, originalPrice: 99, sales: 120000, category: '服饰鞋包', emoji: '👕', tags: ['包邮', '爆款'], shop: '优衣库官方旗舰店', desc: '100%精梳棉，亲肤透气，多色可选。', detail: '优衣库纯棉圆领T恤，采用100%精梳棉，柔软亲肤，透气舒适。罗纹圆领，不易变形。男女同款，多色可选，日常百搭必备。' },
  { id: 8, name: 'SK-II神仙水护肤精华露 230ml 油皮救星 提亮修护', price: 1690, originalPrice: 2190, sales: 8900, category: '美妆护肤', emoji: '🧴', tags: ['限量', '天猫'], shop: 'SK-II官方旗舰店', desc: 'PITERA™精华，调节水油平衡，提亮肤色。', detail: 'SK-II神仙水，蕴含90%以上PITERA™天然酵母精华，调节肌肤水油平衡，改善毛孔粗大，让肌肤晶莹剔透。油皮亲妈，连续使用效果更佳。' },
  { id: 9, name: '三只松鼠每日坚果750g/30包 混合坚果果仁孕妇零食', price: 129, originalPrice: 169, sales: 56000, category: '食品生鲜', emoji: '🥜', tags: ['包邮'], shop: '三只松鼠旗舰店', desc: '30小包装，每天一包，营养均衡。', detail: '三只松鼠每日坚果，750g共30包，每天一包刚刚好。内含6种坚果+果干，科学配比，营养均衡。孕妇、儿童、上班族都适合。' },
  { id: 10, name: '小米扫地机器人2 Pro 全自动家用扫拖一体 智能导航', price: 1999, originalPrice: 2499, sales: 15000, category: '家居家装', emoji: '🏠', tags: ['热销', '包邮'], shop: '小米官方旗舰店', desc: 'LDS激光导航，扫拖一体，自动回充。', detail: '小米扫地机器人2 Pro，LDS激光导航，精准建图。扫拖一体，2500Pa大吸力。自动回充，APP远程控制。手机操控，解放双手。' },
  { id: 11, name: 'Sony WH-1000XM5 头戴式降噪耳机 无线蓝牙 通勤旗舰', price: 2499, originalPrice: 2899, sales: 9800, category: '手机数码', emoji: '🎧', tags: ['新品', '天猫'], shop: 'Sony索尼旗舰店', desc: '旗舰降噪，30小时续航，Hi-Res音质。', detail: 'Sony WH-1000XM5，新一代降噪芯片，降噪效果提升40%。30小时超长续航，快速充电。Hi-Res金标认证，音质出色。通勤、旅行必备。' },
  { id: 12, name: 'Adidas三叶草卫衣 男女同款 春秋宽松运动休闲套头衫', price: 499, originalPrice: 699, sales: 23000, category: '服饰鞋包', emoji: '👕', tags: ['特价', '包邮'], shop: 'adidas官方旗舰店', desc: '纯棉毛圈布，宽松版型，三叶草经典logo。', detail: 'Adidas三叶草卫衣，采用纯棉毛圈布面料，柔软舒适。宽松落肩版型，休闲百搭。胸前三叶草经典logo，复古潮流。男女同款。' },
])

// 购物车
const cart = ref([])
// 收藏
const favorites = ref([])
// 关注店铺
const followedShops = ref([])
// 订单
const orders = ref([])

// 筛选商品
const filteredProducts = computed(() => {
  let list = products.value
  if (activeCategory.value !== '全部') list = list.filter(p => p.category === activeCategory.value)
  return list
})

const cartCount = computed(() => cart.value.reduce((s, i) => s + i.quantity, 0))
const cartTotal = computed(() => cart.value.reduce((s, i) => s + i.price * i.quantity, 0))
const favoriteProducts = computed(() => products.value.filter(p => favorites.value.includes(p.id)))

function goPage(page, product = null) {
  currentPage.value = page
  if (product) currentProduct.value = product
  window.scrollTo(0, 0)
}

function addToCart(product) {
  const existing = cart.value.find(i => i.id === product.id)
  if (existing) existing.quantity++
  else cart.value.push({ ...product, quantity: 1 })
}

function decreaseQuantity(id) {
  const item = cart.value.find(i => i.id === id)
  if (item) {
    item.quantity--
    if (item.quantity <= 0) cart.value = cart.value.filter(i => i.id !== id)
  }
}

function removeFromCart(id) {
  cart.value = cart.value.filter(i => i.id !== id)
}

function toggleFavorite(id) {
  if (favorites.value.includes(id)) favorites.value = favorites.value.filter(i => i !== id)
  else favorites.value.push(id)
}

function toggleFollowShop(shopName) {
  if (followedShops.value.includes(shopName)) followedShops.value = followedShops.value.filter(s => s !== shopName)
  else followedShops.value.push(shopName)
}

function checkout() {
  orders.value.push({
    id: 'order_' + Date.now(),
    items: [...cart.value],
    total: cartTotal.value,
    time: new Date().toLocaleString()
  })
  cart.value = []
  goPage('orders')
}
</script>

<template>
  <!-- ========== 首页 ========== -->
  <div v-if="currentPage === 'home'">
    <!-- 顶部导航 -->
    <header class="top-header">
      <div class="header-inner">
        <div class="top-nav">
          <span class="nav-item active">首页</span>
          <span class="nav-item" @click="goPage('live')">淘宝直播</span>
          <span class="nav-item" @click="goPage('flash')">闪购</span>
          <span class="nav-item" @click="goPage('coupon')">领券</span>
          <span class="nav-item" @click="goPage('member')">会员中心</span>
        </div>
      </div>
    </header>

    <!-- 搜索栏 -->
    <div class="search-bar">
      <div class="search-inner">
        <div class="logo" @click="goPage('home')">
          <span class="logo-1">淘</span><span class="logo-2">宝</span>
        </div>
        <div class="search-box">
          <input type="text" placeholder="iPhone 15 Pro" class="search-input" />
          <button class="search-btn">搜索</button>
        </div>
        <div class="header-cart" @click="goPage('cart')">
          <span>🛒</span>
          <span v-if="cartCount > 0" class="cart-badge">{{ cartCount }}</span>
          <span class="cart-text">购物车</span>
        </div>
      </div>
    </div>

    <!-- 快捷入口 -->
    <div class="quick-entry">
      <div class="entry-item" @click="goPage('live')">
        <div class="entry-icon">🔴</div>
        <div class="entry-label">淘宝直播</div>
      </div>
      <div class="entry-item" @click="goPage('flash')">
        <div class="entry-icon">⚡</div>
        <div class="entry-label">闪购</div>
      </div>
      <div class="entry-item" @click="goPage('coupon')">
        <div class="entry-icon">🎫</div>
        <div class="entry-label">领券中心</div>
      </div>
      <div class="entry-item" @click="goPage('home')">
        <div class="entry-icon">🏛️</div>
        <div class="entry-label">国补专区</div>
      </div>
      <div class="entry-item" @click="goPage('favorites')">
        <div class="entry-icon">⭐</div>
        <div class="entry-label">我的收藏</div>
      </div>
      <div class="entry-item" @click="goPage('orders')">
        <div class="entry-icon">📦</div>
        <div class="entry-label">我的订单</div>
      </div>
      <div class="entry-item" @click="goPage('member')">
        <div class="entry-icon">👑</div>
        <div class="entry-label">会员中心</div>
      </div>
    </div>

    <!-- 分类标签 -->
    <div class="category-bar">
      <div class="category-inner">
        <div 
          v-for="cat in categories" :key="cat"
          class="cat-item" :class="{ active: activeCategory === cat }"
          @click="activeCategory = cat"
        >{{ cat }}</div>
      </div>
    </div>

    <!-- 商品列表 -->
    <main class="product-list">
      <div v-for="p in filteredProducts" :key="p.id" class="product-card" @click="goPage('detail', p)">
        <div class="p-image">
          <span class="p-emoji">{{ p.emoji }}</span>
          <span class="p-tag-tmall" v-if="p.tags.includes('天猫')">天猫</span>
        </div>
        <div class="p-info">
          <div class="p-name">{{ p.name }}</div>
          <div class="p-price-row">
            <span class="p-symbol">¥</span>
            <span class="p-price">{{ p.price }}</span>
            <span class="p-original" v-if="p.originalPrice > p.price">¥{{ p.originalPrice }}</span>
          </div>
          <div class="p-sales-row">
            <span class="p-sales">{{ p.sales }}人付款</span>
            <button class="p-add" @click.stop="addToCart(p)">+</button>
          </div>
          <div class="p-shop">{{ p.shop }}</div>
        </div>
      </div>
    </main>

    <!-- 猜你喜欢 -->
    <div class="section-title">猜你喜欢</div>
    <main class="product-list">
      <div v-for="p in products.slice(0, 6)" :key="p.id" class="product-card" @click="goPage('detail', p)">
        <div class="p-image">
          <span class="p-emoji">{{ p.emoji }}</span>
        </div>
        <div class="p-info">
          <div class="p-name">{{ p.name }}</div>
          <div class="p-price-row">
            <span class="p-symbol">¥</span>
            <span class="p-price">{{ p.price }}</span>
          </div>
        </div>
      </div>
    </main>
  </div>

  <!-- ========== 商品详情页 ========== -->
  <div v-else-if="currentPage === 'detail' && currentProduct">
    <div class="detail-top">
      <button class="back-btn" @click="goPage('home')">← 返回</button>
    </div>
    <div class="detail-container">
      <div class="detail-image">
        <span class="big-emoji">{{ currentProduct.emoji }}</span>
      </div>
      <div class="detail-info">
        <div class="detail-price-row">
          <span class="detail-symbol">¥</span>
          <span class="detail-price">{{ currentProduct.price }}</span>
          <span class="detail-original">¥{{ currentProduct.originalPrice }}</span>
        </div>
        <h1 class="detail-name">{{ currentProduct.name }}</h1>
        <div class="detail-sales">{{ currentProduct.sales }}人付款 · {{ currentProduct.shop }}</div>
        
        <div class="detail-desc">
          <h3>商品介绍</h3>
          <p>{{ currentProduct.detail }}</p>
        </div>

        <div class="detail-actions">
          <button class="action-btn fav" @click="toggleFavorite(currentProduct.id)">
            {{ favorites.includes(currentProduct.id) ? '❤️ 已收藏' : '🤍 收藏' }}
          </button>
          <button class="action-btn follow" @click="toggleFollowShop(currentProduct.shop)">
            {{ followedShops.includes(currentProduct.shop) ? '✓ 已关注' : '+ 关注店铺' }}
          </button>
        </div>

        <div class="detail-bottom-bar">
          <button class="bottom-btn cart-btn" @click="addToCart(currentProduct)">加入购物车</button>
          <button class="bottom-btn buy-btn" @click="addToCart(currentProduct); goPage('cart')">立即购买</button>
        </div>
      </div>
    </div>
  </div>

  <!-- ========== 购物车页 ========== -->
  <div v-else-if="currentPage === 'cart'">
    <div class="page-header">
      <button class="back-btn" @click="goPage('home')">← 返回</button>
      <h2>购物车 ({{ cartCount }})</h2>
    </div>
    <div class="cart-page">
      <div v-if="cart.length === 0" class="empty-state">
        <div class="empty-icon">🛒</div>
        <p>购物车还是空的</p>
        <button class="primary-btn" @click="goPage('home')">去逛逛</button>
      </div>
      <div v-else>
        <div v-for="item in cart" :key="item.id" class="cart-item">
          <span class="ci-emoji">{{ item.emoji }}</span>
          <div class="ci-info">
            <div class="ci-name">{{ item.name }}</div>
            <div class="ci-price">¥{{ item.price }}</div>
          </div>
          <div class="ci-qty">
            <button @click="decreaseQuantity(item.id)">-</button>
            <span>{{ item.quantity }}</span>
            <button @click="addToCart(item)">+</button>
          </div>
          <button class="ci-del" @click="removeFromCart(item.id)">删除</button>
        </div>
        <div class="cart-footer">
          <div class="total-row">
            <span>合计：</span>
            <span class="total-price">¥{{ cartTotal.toFixed(2) }}</span>
          </div>
          <button class="checkout-btn" @click="checkout()">去结算 ({{ cartCount }})</button>
        </div>
      </div>
    </div>
  </div>

  <!-- ========== 我的订单页 ========== -->
  <div v-else-if="currentPage === 'orders'">
    <div class="page-header">
      <button class="back-btn" @click="goPage('home')">← 返回</button>
      <h2>我的订单</h2>
    </div>
    <div class="list-page">
      <div v-if="orders.length === 0" class="empty-state">
        <div class="empty-icon">📦</div>
        <p>还没有订单</p>
        <button class="primary-btn" @click="goPage('home')">去购物</button>
      </div>
      <div v-else>
        <div v-for="order in orders" :key="order.id" class="order-card">
          <div class="order-head">
            <span>订单号：{{ order.id }}</span>
            <span class="order-time">{{ order.time }}</span>
          </div>
          <div v-for="item in order.items" :key="item.id" class="order-item">
            <span>{{ item.emoji }} {{ item.name }} × {{ item.quantity }}</span>
            <span>¥{{ item.price * item.quantity }}</span>
          </div>
          <div class="order-total">合计：¥{{ order.total.toFixed(2) }}</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ========== 会员中心 ========== -->
  <div v-else-if="currentPage === 'member'">
    <div class="page-header">
      <button class="back-btn" @click="goPage('home')">← 返回</button>
      <h2>会员中心</h2>
    </div>
    <div class="member-page">
      <div class="member-card">
        <div class="member-level">👑 黄金会员</div>
        <div class="member-points">当前积分：1280</div>
        <div class="member-progress">再消费720元升级铂金会员</div>
      </div>
      <div class="member-grid">
        <div class="member-entry" @click="goPage('orders')">
          <div class="me-icon">📦</div>
          <div>我的订单</div>
        </div>
        <div class="member-entry" @click="goPage('favorites')">
          <div class="me-icon">⭐</div>
          <div>我的收藏</div>
        </div>
        <div class="member-entry" @click="goPage('coupon')">
          <div class="me-icon">🎫</div>
          <div>我的优惠券</div>
        </div>
        <div class="member-entry" @click="goPage('home')">
          <div class="me-icon">👤</div>
          <div>个人资料</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ========== 我的收藏 ========== -->
  <div v-else-if="currentPage === 'favorites'">
    <div class="page-header">
      <button class="back-btn" @click="goPage('home')">← 返回</button>
      <h2>我的收藏 ({{ favorites.length }})</h2>
    </div>
    <div class="list-page">
      <div v-if="favoriteProducts.length === 0" class="empty-state">
        <div class="empty-icon">⭐</div>
        <p>还没有收藏商品</p>
        <button class="primary-btn" @click="goPage('home')">去逛逛</button>
      </div>
      <div v-else class="product-list">
        <div v-for="p in favoriteProducts" :key="p.id" class="product-card" @click="goPage('detail', p)">
          <div class="p-image">
            <span class="p-emoji">{{ p.emoji }}</span>
          </div>
          <div class="p-info">
            <div class="p-name">{{ p.name }}</div>
            <div class="p-price-row">
              <span class="p-symbol">¥</span>
              <span class="p-price">{{ p.price }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ========== 淘宝直播 ========== -->
  <div v-else-if="currentPage === 'live'">
    <div class="page-header">
      <button class="back-btn" @click="goPage('home')">← 返回</button>
      <h2>淘宝直播</h2>
    </div>
    <div class="list-page">
      <div class="live-grid">
        <div class="live-card" v-for="i in 6" :key="i">
          <div class="live-cover">
            <span class="live-emoji">🎬</span>
            <span class="live-badge">直播中</span>
          </div>
          <div class="live-title">直播间标题 {{ i }}</div>
          <div class="live-viewers">{{ (i * 1200 + 500).toLocaleString() }}人观看</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ========== 闪购/领券 ========== -->
  <div v-else-if="currentPage === 'flash' || currentPage === 'coupon'">
    <div class="page-header">
      <button class="back-btn" @click="goPage('home')">← 返回</button>
      <h2>{{ currentPage === 'flash' ? '限时闪购' : '领券中心' }}</h2>
    </div>
    <div class="list-page">
      <div class="coupon-list">
        <div v-for="i in 6" :key="i" class="coupon-card">
          <div class="coupon-left">
            <div class="coupon-amount">¥{{ i * 10 }}</div>
            <div class="coupon-condition">满{{ i * 50 }}可用</div>
          </div>
          <div class="coupon-right">
            <div class="coupon-name">{{ currentPage === 'flash' ? '限时闪购特惠' : '店铺优惠券' }} {{ i }}</div>
            <button class="coupon-btn">立即领取</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  font-family: -apple-system, "PingFang SC", "Microsoft YaHei", sans-serif;
  background: #f4f4f4;
  color: #333;
}

/* 顶部导航 */
.top-header { background: #ff4400; height: 36px; }
.header-inner { max-width: 1200px; margin: 0 auto; padding: 0 20px; }
.top-nav { display: flex; gap: 20px; line-height: 36px; }
.nav-item { font-size: 12px; color: rgba(255,255,255,0.9); cursor: pointer; }
.nav-item.active { color: #fff; font-weight: 600; }

/* 搜索栏 */
.search-bar { background: #ff4400; padding-bottom: 12px; }
.search-inner { max-width: 1200px; margin: 0 auto; padding: 0 20px; display: flex; align-items: center; gap: 16px; }
.logo { font-size: 24px; font-weight: 800; cursor: pointer; }
.logo-1 { color: #fff; }
.logo-2 { color: #ff4400; background: #fff; padding: 2px 4px; border-radius: 3px; margin-left: 2px; }
.search-box { flex: 1; display: flex; background: #fff; border-radius: 18px; overflow: hidden; height: 36px; }
.search-input { flex: 1; border: none; padding: 0 16px; font-size: 14px; outline: none; }
.search-btn { background: #ff4400; color: #fff; border: none; padding: 0 28px; font-size: 14px; font-weight: 600; cursor: pointer; }
.header-cart { display: flex; align-items: center; gap: 4px; color: #fff; cursor: pointer; position: relative; padding: 6px 12px; border-radius: 18px; background: rgba(255,255,255,0.15); }
.cart-text { font-size: 13px; }
.cart-badge { position: absolute; top: -2px; right: 8px; background: #fff; color: #ff4400; font-size: 11px; font-weight: 700; padding: 1px 5px; border-radius: 8px; min-width: 16px; text-align: center; }

/* 快捷入口 */
.quick-entry { background: #fff; padding: 16px 20px; display: flex; justify-content: space-around; }
.entry-item { text-align: center; cursor: pointer; }
.entry-icon { font-size: 28px; margin-bottom: 4px; }
.entry-label { font-size: 12px; color: #666; }

/* 分类栏 */
.category-bar { background: #fff; border-bottom: 1px solid #f0f0f0; }
.category-inner { max-width: 1200px; margin: 0 auto; padding: 0 20px; display: flex; gap: 8px; overflow-x: auto; }
.cat-item { padding: 12px 16px; font-size: 14px; color: #666; cursor: pointer; white-space: nowrap; border-bottom: 2px solid transparent; }
.cat-item.active { color: #ff4400; border-bottom-color: #ff4400; font-weight: 600; }

/* 商品列表 */
.product-list { max-width: 1200px; margin: 16px auto; padding: 0 20px; display: grid; grid-template-columns: repeat(auto-fill, minmax(210px, 1fr)); gap: 12px; }
.product-card { background: #fff; border-radius: 8px; overflow: hidden; cursor: pointer; transition: transform 0.15s; }
.product-card:hover { transform: translateY(-2px); }
.p-image { height: 180px; background: linear-gradient(180deg, #fff8f5 0%, #ffe8e0 100%); display: flex; align-items: center; justify-content: center; position: relative; }
.p-emoji { font-size: 72px; }
.p-tag-tmall { position: absolute; top: 8px; left: 8px; background: #ff0036; color: #fff; font-size: 10px; padding: 2px 6px; border-radius: 3px; font-weight: 600; }
.p-info { padding: 10px; }
.p-name { font-size: 13px; line-height: 1.4; color: #333; height: 36px; overflow: hidden; margin-bottom: 6px; }
.p-price-row { display: flex; align-items: baseline; gap: 6px; margin-bottom: 4px; }
.p-symbol { font-size: 12px; color: #ff4400; font-weight: 600; }
.p-price { font-size: 20px; font-weight: 700; color: #ff4400; }
.p-original { font-size: 11px; color: #bbb; text-decoration: line-through; }
.p-sales-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px; }
.p-sales { font-size: 11px; color: #999; }
.p-add { width: 26px; height: 26px; border-radius: 50%; background: #ff4400; color: #fff; border: none; font-size: 16px; cursor: pointer; }
.p-shop { font-size: 11px; color: #999; }

/* 区块标题 */
.section-title { max-width: 1200px; margin: 24px 20px 12px; font-size: 16px; font-weight: 600; color: #333; }

/* 详情页 */
.detail-top { background: #fff; padding: 12px 20px; border-bottom: 1px solid #f0f0f0; }
.back-btn { background: none; border: none; font-size: 14px; color: #ff4400; cursor: pointer; }
.detail-container { max-width: 1000px; margin: 20px auto; padding: 0 20px; display: grid; grid-template-columns: 1fr 1fr; gap: 30px; }
.detail-image { background: linear-gradient(180deg, #fff8f5 0%, #ffe8e0 100%); border-radius: 12px; height: 400px; display: flex; align-items: center; justify-content: center; }
.big-emoji { font-size: 160px; }
.detail-price-row { display: flex; align-items: baseline; gap: 8px; margin-bottom: 12px; }
.detail-symbol { font-size: 16px; color: #ff4400; font-weight: 600; }
.detail-price { font-size: 36px; font-weight: 700; color: #ff4400; }
.detail-original { font-size: 14px; color: #bbb; text-decoration: line-through; }
.detail-name { font-size: 20px; line-height: 1.4; margin-bottom: 8px; }
.detail-sales { font-size: 13px; color: #999; margin-bottom: 20px; }
.detail-desc { margin-bottom: 24px; }
.detail-desc h3 { font-size: 15px; margin-bottom: 8px; }
.detail-desc p { font-size: 14px; line-height: 1.6; color: #555; }
.detail-actions { display: flex; gap: 12px; margin-bottom: 30px; }
.action-btn { padding: 8px 16px; border: 1px solid #ddd; background: #fff; border-radius: 20px; font-size: 13px; cursor: pointer; }
.detail-bottom-bar { display: flex; gap: 12px; position: sticky; bottom: 20px; }
.bottom-btn { flex: 1; padding: 14px; border: none; border-radius: 24px; font-size: 15px; font-weight: 600; cursor: pointer; }
.cart-btn { background: #ffb800; color: #fff; }
.buy-btn { background: #ff4400; color: #fff; }

/* 页面通用 */
.page-header { background: #fff; padding: 12px 20px; border-bottom: 1px solid #f0f0f0; display: flex; align-items: center; gap: 16px; }
.page-header h2 { font-size: 18px; }
.list-page { max-width: 1000px; margin: 20px auto; padding: 0 20px; }
.empty-state { text-align: center; padding: 60px 20px; color: #999; }
.empty-icon { font-size: 48px; margin-bottom: 12px; }
.primary-btn { margin-top: 16px; background: #ff4400; color: #fff; border: none; padding: 10px 24px; border-radius: 20px; font-size: 14px; cursor: pointer; }

/* 购物车 */
.cart-page { max-width: 1000px; margin: 20px auto; padding: 0 20px; }
.cart-item { display: flex; align-items: center; gap: 12px; background: #fff; padding: 16px; border-radius: 8px; margin-bottom: 10px; }
.ci-emoji { font-size: 40px; }
.ci-info { flex: 1; }
.ci-name { font-size: 14px; margin-bottom: 4px; }
.ci-price { font-size: 16px; font-weight: 600; color: #ff4400; }
.ci-qty { display: flex; align-items: center; gap: 8px; }
.ci-qty button { width: 24px; height: 24px; border: 1px solid #ddd; background: #fff; border-radius: 4px; cursor: pointer; }
.ci-del { background: none; border: none; color: #999; font-size: 13px; cursor: pointer; }
.cart-footer { background: #fff; padding: 16px; border-radius: 8px; display: flex; justify-content: space-between; align-items: center; margin-top: 16px; }
.total-row { font-size: 15px; }
.total-price { font-size: 24px; font-weight: 700; color: #ff4400; }
.checkout-btn { background: #ff4400; color: #fff; border: none; padding: 12px 32px; border-radius: 24px; font-size: 15px; font-weight: 600; cursor: pointer; }

/* 订单 */
.order-card { background: #fff; border-radius: 8px; padding: 16px; margin-bottom: 12px; }
.order-head { display: flex; justify-content: space-between; font-size: 12px; color: #999; margin-bottom: 12px; padding-bottom: 8px; border-bottom: 1px solid #f5f5f5; }
.order-item { display: flex; justify-content: space-between; padding: 6px 0; font-size: 14px; }
.order-total { text-align: right; margin-top: 10px; font-weight: 600; color: #ff4400; }

/* 会员中心 */
.member-page { max-width: 1000px; margin: 20px auto; padding: 0 20px; }
.member-card { background: linear-gradient(135deg, #ff4400, #ff6600); color: #fff; border-radius: 12px; padding: 24px; margin-bottom: 20px; }
.member-level { font-size: 20px; font-weight: 700; margin-bottom: 8px; }
.member-points { font-size: 14px; opacity: 0.9; margin-bottom: 8px; }
.member-progress { font-size: 12px; opacity: 0.8; }
.member-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; }
.member-entry { background: #fff; border-radius: 8px; padding: 20px; text-align: center; cursor: pointer; }
.me-icon { font-size: 28px; margin-bottom: 8px; }

/* 直播 */
.live-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 16px; }
.live-card { background: #fff; border-radius: 8px; overflow: hidden; cursor: pointer; }
.live-cover { height: 180px; background: linear-gradient(135deg, #1a1a2e, #16213e); display: flex; align-items: center; justify-content: center; position: relative; }
.live-emoji { font-size: 60px; }
.live-badge { position: absolute; top: 10px; left: 10px; background: #ff0036; color: #fff; font-size: 11px; padding: 2px 8px; border-radius: 4px; }
.live-title { padding: 10px 12px; font-size: 14px; }
.live-viewers { padding: 0 12px 12px; font-size: 12px; color: #999; }

/* 优惠券 */
.coupon-list { display: flex; flex-direction: column; gap: 12px; }
.coupon-card { background: #fff; border-radius: 12px; display: flex; overflow: hidden; }
.coupon-left { background: linear-gradient(135deg, #ff4400, #ff6600); color: #fff; padding: 20px; width: 120px; display: flex; flex-direction: column; justify-content: center; }
.coupon-amount { font-size: 28px; font-weight: 700; }
.coupon-condition { font-size: 12px; opacity: 0.9; }
.coupon-right { flex: 1; padding: 20px; display: flex; justify-content: space-between; align-items: center; }
.coupon-name { font-size: 14px; }
.coupon-btn { background: #ff4400; color: #fff; border: none; padding: 8px 20px; border-radius: 20px; font-size: 13px; cursor: pointer; }
</style>
