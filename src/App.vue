<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import SiteHeader from './components/SiteHeader.vue'
import HeroBanner from './components/HeroBanner.vue'
import PlatformTabs from './components/PlatformTabs.vue'
import CategorySidebar from './components/CategorySidebar.vue'
import SiteFooter from './components/SiteFooter.vue'
import RightFloatNav from './components/RightFloatNav.vue'
import LoginPage from './pages/LoginPage.vue'
import RegisterPage from './pages/RegisterPage.vue'
import GettingStartedPage from './pages/GettingStartedPage.vue'
import HelpCenterPage from './pages/HelpCenterPage.vue'
import FAQDetailPage from './pages/FAQDetailPage.vue'
import ContactUsPage from './pages/ContactUsPage.vue'
import AboutSaleyeePage from './pages/AboutSaleyeePage.vue'
import VATPolicyPage from './pages/VATPolicyPage.vue'
import FeedbackPage from './pages/FeedbackPage.vue'
import DistributorsPage from './pages/DistributorsPage.vue'
import SupplierPage from './pages/SupplierPage.vue'
import PartnersPage from './pages/PartnersPage.vue'
import ItemDetailPage from './pages/ItemDetailPage.vue'
import UserCenterPage from './pages/UserCenterPage.vue'
import ProductManagementPage from './pages/ProductManagementPage.vue'
import ListingPushPage from './pages/ListingPushPage.vue'
import ListingProductsPage from './pages/ListingProductsPage.vue'
import BrandAuthPage from './pages/BrandAuthPage.vue'
import ProductNotificationPage from './pages/ProductNotificationPage.vue'
import CartPage from './pages/CartPage.vue'
import { t } from '@/i18n'

const heroHeight = ref<number | undefined>(undefined)
const normalizedPath = window.location.pathname.replace(/\/+$/, '') || '/'
const isLogin = normalizedPath === '/login'
const isRegister = normalizedPath === '/register'
const isGettingStarted = normalizedPath === '/getting-started' || normalizedPath === '/getting-started.html'
const isHelpCenter = normalizedPath === '/help-center' || normalizedPath === '/help-center.html'
const isFAQDetail = normalizedPath.match(/^\/faq\//) !== null
const isContactUs = normalizedPath === '/contact-us' || normalizedPath === '/contact-us.html'
const isAboutSaleyee = normalizedPath === '/about-saleyee' || normalizedPath === '/about-saleyee.html'
const isVATPolicy = normalizedPath === '/vat-policy' || normalizedPath === '/vat-policy.html'
const isFeedback = normalizedPath === '/feedback' || normalizedPath === '/feedback.html'
const isDistributors = normalizedPath === '/distributors' || normalizedPath === '/distributors.html'
const isSupplier = normalizedPath === '/supplier' || normalizedPath === '/supplier.html'
const isPartners = normalizedPath === '/partners' || normalizedPath === '/partners.html'
const isItemDetail = normalizedPath.match(/^\/item\//) !== null
const isUserCenter = normalizedPath === '/user-center' || normalizedPath === '/user-center.html'
const isProductManagement = normalizedPath === '/product-management' || normalizedPath === '/product-management.html'
const isListingPush = normalizedPath === '/listing-push' || normalizedPath === '/listing-push.html'
const isListingProducts = normalizedPath === '/listing-products' || normalizedPath === '/listing-products.html'
const isBrandAuth = normalizedPath === '/brand-auth' || normalizedPath === '/brand-auth.html'
const isProductNotification = normalizedPath === '/product-notification' || normalizedPath === '/product-notification.html'
const isCart = normalizedPath === '/cart' || normalizedPath === '/cart.html'

const newImages = [
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202309/2a764166-da43-45a1-9fba-ceea6243b6b7.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2025/202501/8829a6ab-0a10-4349-a506-12835c30c872.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2025/202506/89c6cc05-8416-491e-8b0e-3fde8a318f0e.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202309/3c1b039b-de57-4d8f-805a-4d15658b90c5.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202310/3c1b039b-de57-4d8f-805a-4d15658b90c5.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202309/79ebe6df-c2d3-4207-b146-513630fe163d.Jpeg',
]

const regionImages = [
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2024/202410/890f8f68-c40a-40c5-a4f3-017cf297c900.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2025/202508/d41d793d-cf75-4653-8470-a715d6e9f12f.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2025/202508/7656a659-2d14-4c49-a355-c45b0ae7edf8.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202308/9edd9fdd-b8ca-4750-8c74-6180331317f0.jpg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2025/202506/89c6cc05-8416-491e-8b0e-3fde8a318f0e.Jpeg',
  'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202309/3c1b039b-de57-4d8f-805a-4d15658b90c5.Jpeg',
]

const fallbackImage = 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202310/3c1b039b-de57-4d8f-805a-4d15658b90c5.Jpeg'
function onImgError(e: Event) {
  const t = e.target as HTMLImageElement
  if (t && t.src !== fallbackImage) t.src = fallbackImage
}

function updateHeroHeight() {
  const el = document.querySelector('.category-sidebar') as HTMLElement | null
  if (el && window.innerWidth >= 768) {
    // use fixed height to match Saleyee homepage carousel
    heroHeight.value = 460
  } else {
    heroHeight.value = undefined
  }
}

onMounted(() => {
  updateHeroHeight()
  window.addEventListener('resize', updateHeroHeight)
})

onUnmounted(() => window.removeEventListener('resize', updateHeroHeight))
</script>

<template>
  <LoginPage v-if="isLogin" />
  <RegisterPage v-else-if="isRegister" />
  <GettingStartedPage v-else-if="isGettingStarted" />
  <HelpCenterPage v-else-if="isHelpCenter" />
  <FAQDetailPage v-else-if="isFAQDetail" />
  <ContactUsPage v-else-if="isContactUs" />
  <AboutSaleyeePage v-else-if="isAboutSaleyee" />
  <VATPolicyPage v-else-if="isVATPolicy" />
  <FeedbackPage v-else-if="isFeedback" />
  <DistributorsPage v-else-if="isDistributors" />
  <SupplierPage v-else-if="isSupplier" />
  <PartnersPage v-else-if="isPartners" />
  <ItemDetailPage v-else-if="isItemDetail" />
  <UserCenterPage v-else-if="isUserCenter" />
  <ProductManagementPage v-else-if="isProductManagement" />
  <ListingPushPage v-else-if="isListingPush" />
  <ListingProductsPage v-else-if="isListingProducts" />
  <BrandAuthPage v-else-if="isBrandAuth" />
  <ProductNotificationPage v-else-if="isProductNotification" />
  <CartPage v-else-if="isCart" />
  <div v-else id="top" class="min-h-screen flex flex-col">
    <SiteHeader />
    <main class="flex-1">
      <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
        <el-row :gutter="0">
          <el-col :xs="0" :md="6" class="hidden md:block">
            <CategorySidebar />
          </el-col>
          <el-col :xs="24" :md="18" class="md:overflow-visible">
            <div class="w-full md:ml-[-90px] md:w-[calc(100%+90px)]">
              <HeroBanner :heroHeight="heroHeight" />
            </div>
          </el-col>
        </el-row>
      </div>

      <PlatformTabs />

      <!-- 平台精选 -->
      <section id="platform" class="bg-white py-8">
        <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
          <h2 class="text-xl md:text-2xl font-semibold text-slate-900 mb-6 text-center">平台精选</h2>
          <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4">
            <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition p-4">
              <div class="aspect-[4/3] bg-slate-50 overflow-hidden rounded mb-2">
                <img src="https://via.placeholder.com/300x225?text=Amazon" alt="Amazon" @error="onImgError" class="h-full w-full object-contain" />
              </div>
              <p class="text-sm text-slate-700 text-center">Amazon 精选商品</p>
            </a>
            <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition p-4">
              <div class="aspect-[4/3] bg-slate-50 overflow-hidden rounded mb-2">
                <img src="https://via.placeholder.com/300x225?text=eBay" alt="eBay" @error="onImgError" class="h-full w-full object-contain" />
              </div>
              <p class="text-sm text-slate-700 text-center">eBay 热销产品</p>
            </a>
            <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition p-4">
              <div class="aspect-[4/3] bg-slate-50 overflow-hidden rounded mb-2">
                <img src="https://via.placeholder.com/300x225?text=TEMU" alt="TEMU" @error="onImgError" class="h-full w-full object-contain" />
              </div>
              <p class="text-sm text-slate-700 text-center">TEMU 爆款推荐</p>
            </a>
            <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition p-4">
              <div class="aspect-[4/3] bg-slate-50 overflow-hidden rounded mb-2">
                <img src="https://via.placeholder.com/300x225?text=SHEIN" alt="SHEIN" @error="onImgError" class="h-full w-full object-contain" />
              </div>
              <p class="text-sm text-slate-700 text-center">SHEIN 时尚好物</p>
            </a>
            <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition p-4">
              <div class="aspect-[4/3] bg-slate-50 overflow-hidden rounded mb-2">
                <img src="https://via.placeholder.com/300x225?text=TikTok" alt="TikTok" @error="onImgError" class="h-full w-full object-contain" />
              </div>
              <p class="text-sm text-slate-700 text-center">TikTok 热门商品</p>
            </a>
          </div>
        </div>
      </section>

      <!-- 节日装饰 -->
      <section id="festival" class="bg-slate-50 py-8">
        <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
          <h2 class="text-xl md:text-2xl font-semibold text-slate-900 mb-6 text-center">节日装饰</h2>
          <div class="grid grid-cols-1 lg:grid-cols-6 gap-4">
            <!-- 左侧大图广告 -->
            <div class="lg:col-span-1">
              <a href="#" class="block rounded overflow-hidden">
                <img src="https://via.placeholder.com/200x600?text=Halloween" alt="万圣节专场" class="w-full h-full object-cover" />
              </a>
            </div>
            <!-- 右侧商品网格 -->
            <div class="lg:col-span-5 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-4">
              <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=Product" alt="商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">万圣节装饰品</p>
                </div>
              </a>
              <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=Product" alt="商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">圣诞装饰花环</p>
                </div>
              </a>
              <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=Product" alt="商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">圣诞树</p>
                </div>
              </a>
              <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=Product" alt="商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">节日灯饰</p>
                </div>
              </a>
              <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=Product" alt="商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">圣诞老人摆件</p>
                </div>
              </a>
              <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=Product" alt="商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">圣诞挂饰</p>
                </div>
              </a>
              <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=Product" alt="商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">节日礼品袋</p>
                </div>
              </a>
              <a href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=Product" alt="商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">节日派对用品</p>
                </div>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- 新品推荐 -->
      <section id="new" class="bg-white py-8">
        <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
          <h2 class="text-xl md:text-2xl font-semibold text-slate-900 mb-6 text-center">新品推荐</h2>
          <div class="grid grid-cols-1 lg:grid-cols-6 gap-4">
            <!-- 左侧大图广告 -->
            <div class="lg:col-span-1">
              <a href="#" class="block rounded overflow-hidden">
                <img src="https://via.placeholder.com/200x600?text=NewArrivals" alt="新品上市" class="w-full h-full object-cover" />
              </a>
            </div>
            <!-- 右侧商品网格 -->
            <div class="lg:col-span-5 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4">
              <a v-for="i in 10" :key="'new-' + i" href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=NewProduct" alt="新品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">新品推荐商品 {{ i }}</p>
                </div>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- 居家推荐 -->
      <section id="home" class="bg-slate-50 py-8">
        <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
          <h2 class="text-xl md:text-2xl font-semibold text-slate-900 mb-6 text-center">居家推荐</h2>
          <div class="grid grid-cols-1 lg:grid-cols-6 gap-4">
            <!-- 左侧大图广告 -->
            <div class="lg:col-span-1">
              <a href="#" class="block rounded overflow-hidden">
                <img src="https://via.placeholder.com/200x600?text=HomeDecor" alt="居家装饰" class="w-full h-full object-cover" />
              </a>
            </div>
            <!-- 右侧商品网格 -->
            <div class="lg:col-span-5 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4">
              <a v-for="i in 10" :key="'home-' + i" href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=HomeProduct" alt="居家商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">居家推荐商品 {{ i }}</p>
                </div>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- 节庆产品 -->
      <section id="seasonal" class="bg-white py-8">
        <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
          <h2 class="text-xl md:text-2xl font-semibold text-slate-900 mb-6 text-center">节庆产品</h2>
          <div class="grid grid-cols-1 lg:grid-cols-6 gap-4">
            <!-- 左侧大图广告 -->
            <div class="lg:col-span-1">
              <a href="#" class="block rounded overflow-hidden">
                <img src="https://via.placeholder.com/200x600?text=Seasonal" alt="节庆" class="w-full h-full object-cover" />
              </a>
            </div>
            <!-- 右侧商品网格 -->
            <div class="lg:col-span-5 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4">
              <a v-for="i in 10" :key="'seasonal-' + i" href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=SeasonalProduct" alt="节庆商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">节庆产品 {{ i }}</p>
                </div>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- 平台产品 -->
      <section id="platform-products" class="bg-slate-50 py-8">
        <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
          <h2 class="text-xl md:text-2xl font-semibold text-slate-900 mb-6 text-center">平台产品</h2>
          <div class="grid grid-cols-1 lg:grid-cols-6 gap-4">
            <!-- 左侧大图广告 -->
            <div class="lg:col-span-1">
              <a href="#" class="block rounded overflow-hidden">
                <img src="https://via.placeholder.com/200x600?text=Platform" alt="平台产品" class="w-full h-full object-cover" />
              </a>
            </div>
            <!-- 右侧商品网格 -->
            <div class="lg:col-span-5 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4">
              <a v-for="i in 10" :key="'platform-' + i" href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=PlatformProduct" alt="平台商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">平台产品 {{ i }}</p>
                </div>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- 家居爆款热销精选 -->
      <section id="bestsellers" class="bg-white py-8">
        <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
          <h2 class="text-xl md:text-2xl font-semibold text-slate-900 mb-6 text-center">家居爆款热销精选</h2>
          <div class="grid grid-cols-1 lg:grid-cols-6 gap-4">
            <!-- 左侧大图广告 -->
            <div class="lg:col-span-1">
              <a href="#" class="block rounded overflow-hidden">
                <img src="https://via.placeholder.com/200x600?text=Bestsellers" alt="爆款" class="w-full h-full object-cover" />
              </a>
            </div>
            <!-- 右侧商品网格 -->
            <div class="lg:col-span-5 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4">
              <a v-for="i in 10" :key="'bestseller-' + i" href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=BestsellerProduct" alt="爆款商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">爆款商品 {{ i }}</p>
                </div>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- 品牌热品大牌联盟 -->
      <section id="brands" class="bg-slate-50 py-8">
        <div class="mx-auto w-full max-w-[1500px] md:w-[80%] md:min-w-[1200px] px-4 md:px-0">
          <h2 class="text-xl md:text-2xl font-semibold text-slate-900 mb-6 text-center">品牌热品大牌联盟</h2>
          <div class="grid grid-cols-1 lg:grid-cols-6 gap-4">
            <!-- 左侧大图广告 -->
            <div class="lg:col-span-1">
              <a href="#" class="block rounded overflow-hidden">
                <img src="https://via.placeholder.com/200x600?text=Brands" alt="品牌" class="w-full h-full object-cover" />
              </a>
            </div>
            <!-- 右侧商品网格 -->
            <div class="lg:col-span-5 grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4">
              <a v-for="i in 10" :key="'brand-' + i" href="#" class="group block bg-white border border-slate-200 rounded hover:shadow-lg transition">
                <div class="aspect-square bg-slate-100 overflow-hidden">
                  <img src="https://via.placeholder.com/300?text=BrandProduct" alt="品牌商品" @error="onImgError" class="h-full w-full object-cover group-hover:scale-105 transition" />
                </div>
                <div class="p-3">
                  <p class="text-sm text-slate-700 line-clamp-2">品牌商品 {{ i }}</p>
                </div>
              </a>
            </div>
          </div>
        </div>
      </section>
    </main>

    <RightFloatNav />
    <SiteFooter />
  </div>
</template>

<style scoped></style>
