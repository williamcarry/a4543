<template>
  <div class="bg-white rounded-lg border border-slate-200 overflow-hidden">
    <!-- 页面标题和订单类型筛选 tabs -->
    <div class="px-6 py-4 border-b border-slate-200">
      <div class="flex flex-col md:flex-row md:items-start md:justify-between gap-4">
        <div class="flex flex-col">
          <h2 class="text-xl font-semibold text-slate-900">我的订单</h2>
          <!-- 订单类型筛选 tabs -->
          <div class="flex flex-wrap gap-3 mt-4">
            <button 
              @click="filterOrders('all')" 
              :class="[
                'px-6 py-2 text-sm font-medium rounded-md border',
                activeFilter === 'all' 
                  ? 'bg-primary text-white border-primary' 
                  : 'bg-white text-slate-700 border-slate-300 hover:bg-slate-50'
              ]"
            >
              全部
            </button>
            <button 
              @click="filterOrders(' wholesale')" 
              :class="[
                'px-6 py-2 text-sm font-medium rounded-md border',
                activeFilter === ' wholesale' 
                  ? 'bg-purple-500 text-white border-purple-500' 
                  : 'bg-white text-slate-700 border-slate-300 hover:bg-slate-50'
              ]"
            >
              批发
            </button>
            <button 
              @click="filterOrders('dropshipping')" 
              :class="[
                'px-6 py-2 text-sm font-medium rounded-md border',
                activeFilter === 'dropshipping' 
                  ? 'bg-cyan-500 text-white border-cyan-500' 
                  : 'bg-white text-slate-700 border-slate-300 hover:bg-slate-50'
              ]"
            >
              一件代发
            </button>
          </div>
        </div>
        <div class="flex flex-wrap items-start gap-3">
          <div class="relative">
            <input 
              type="text" 
              placeholder="订单号/商品名称/SKU" 
              class="pl-10 pr-4 py-2 border border-slate-300 rounded-md text-sm w-64 focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent"
              v-model="searchKeyword"
              @keyup.enter="searchOrders"
            >
            <svg class="w-5 h-5 text-slate-400 absolute left-3 top-2.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
            </svg>
          </div>
          <button 
            @click="searchOrders"
            class="px-4 py-2 bg-primary text-white text-sm rounded-md hover:bg-primary-dark transition"
          >
            搜索
          </button>
          <button 
            @click="resetFilters"
            class="px-4 py-2 border border-slate-300 text-slate-700 text-sm rounded-md hover:bg-slate-50 transition"
          >
            重置
          </button>
        </div>
      </div>
    </div>
    
    <!-- 公告区域 -->
    <div class="px-6 py-3 bg-yellow-50 border-b border-yellow-200">
      <div class="flex items-start">
        <svg class="w-5 h-5 text-yellow-600 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
        </svg>
        <div class="ml-3">
          <p class="text-sm text-yellow-800">
            <span class="font-medium">重要提示：</span><br>
            1.订单均为系统自动化推送并发货，若需要拦截订单，请务必在下单后及时申请拦截，实际拦截结果以系统告知为准 ；<br>
            2.状态为配货中的订单实际还未发出，跟踪号有可能变更，对上传跟踪号后不能进行修改的平台，建议在状态为待收货时再将跟踪号导出上传；<br>
            3.若订单有售后问题，请在平台开启售后并以售后页面的讨论结果为最终操作依据，请关注此页面的留言。平台客服工作时间：9:00-18:00（北京时间 周一 到周五）。周六安排客服值班，具体值班时间以实际为准；
          </p>
        </div>
      </div>
    </div>
    
    <!-- 订单状态筛选 -->
    <div class="px-6 py-3 border-b border-slate-200 bg-slate-50">
      <div class="flex flex-wrap gap-2">
        <button 
          @click="filterByStatus('all')" 
          :class="[
            'px-3 py-1 text-xs rounded',
            activeStatusFilter === 'all' 
              ? 'bg-primary text-white' 
              : 'text-slate-700 hover:bg-slate-200'
          ]"
        >
          全部状态
        </button>
        <button 
          @click="filterByStatus('待付款')" 
          :class="[
            'px-3 py-1 text-xs rounded',
            activeStatusFilter === '待付款' 
              ? 'bg-yellow-500 text-white' 
              : 'text-slate-700 hover:bg-slate-200'
          ]"
        >
          待付款
        </button>
        <button 
          @click="filterByStatus('待发货')" 
          :class="[
            'px-3 py-1 text-xs rounded',
            activeStatusFilter === '待发货' 
              ? 'bg-blue-500 text-white' 
              : 'text-slate-700 hover:bg-slate-200'
          ]"
        >
          待发货
        </button>
        <button 
          @click="filterByStatus('已发货')" 
          :class="[
            'px-3 py-1 text-xs rounded',
            activeStatusFilter === '已发货' 
              ? 'bg-indigo-500 text-white' 
              : 'text-slate-700 hover:bg-slate-200'
          ]"
        >
          已发货
        </button>
        <button 
          @click="filterByStatus('已完成')" 
          :class="[
            'px-3 py-1 text-xs rounded',
            activeStatusFilter === '已完成' 
              ? 'bg-green-500 text-white' 
              : 'text-slate-700 hover:bg-slate-200'
          ]"
        >
          已完成
        </button>
        <button 
          @click="filterByStatus('已取消')" 
          :class="[
            'px-3 py-1 text-xs rounded',
            activeStatusFilter === '已取消' 
              ? 'bg-red-500 text-white' 
              : 'text-slate-700 hover:bg-slate-200'
          ]"
        >
          已取消
        </button>
      </div>
    </div>
    
    <!-- 表格头部 -->
    <div class="overflow-x-auto">
      <table class="min-w-full divide-y divide-slate-200">
        <thead class="bg-slate-50">
          <tr>
            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">商品信息</th>
            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">单价</th>
            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">数量</th>
            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">金额</th>
            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">订单类型</th>
            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">订单状态</th>
            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">操作</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-slate-200">
          <tr v-for="item in paginatedOrders" :key="item.id" class="hover:bg-slate-50">
            <td class="px-6 py-4">
              <div class="flex items-center">
                <div class="flex-shrink-0 h-16 w-16">
                  <img class="h-16 w-16 object-contain" :src="item.productImage" :alt="item.productName" />
                </div>
                <div class="ml-4">
                  <div class="text-sm font-medium text-slate-900">{{ item.productName }}</div>
                  <div class="text-sm text-slate-500">SKU: {{ item.productSku }}</div>
                </div>
              </div>
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              <div class="text-sm text-slate-900">${{ item.unitPrice }}</div>
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              <div class="text-sm text-slate-900">{{ item.quantity }}</div>
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              <div class="text-sm text-slate-900 font-medium">${{ item.totalAmount }}</div>
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full" :class="getOrderTypeClass(item.orderType)">
                {{ item.orderType === ' wholesale' ? '批发' : '一件代发' }}
              </span>
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full" :class="getStatusClass(item.status)">
                {{ item.status }}
              </span>
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-500">
              <button @click="viewDetail(item.id)" class="text-primary hover:text-primary-dark">查看</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- 空状态 -->
    <div v-if="orders.length === 0" class="text-center py-12">
      <div class="text-slate-500">暂无订单数据</div>
    </div>

    <!-- 分页 -->
    <div v-if="orders.length > 0" class="flex items-center justify-between px-6 py-4 border-t border-slate-200 bg-slate-50">
      <div class="text-sm text-slate-600">
        共 {{ totalOrders }} 条 | 第 {{ currentPage }} 页
      </div>
      <div class="flex items-center gap-2">
        <button
          @click="previousPage"
          :disabled="currentPage === 1"
          class="px-3 py-1 text-sm border border-slate-300 rounded hover:bg-white disabled:opacity-50 disabled:cursor-not-allowed transition"
        >
          上一页
        </button>
        <div class="flex items-center gap-1">
          <button
            v-for="page in paginationRange"
            :key="page"
            @click="goToPage(page)"
            :class="[
              'px-3 py-1 text-sm rounded transition',
              currentPage === page
                ? 'bg-primary text-white border border-primary'
                : 'border border-slate-300 hover:bg-slate-100'
            ]"
          >
            {{ page }}
          </button>
        </div>
        <button
          @click="nextPage"
          :disabled="currentPage === totalPages"
          class="px-3 py-1 text-sm border border-slate-300 rounded hover:bg-white disabled:opacity-50 disabled:cursor-not-allowed transition"
        >
          下一页
        </button>
      </div>
    </div>
  </div>

  <!-- 详情弹窗 -->
  <div v-if="showDetailModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-lg max-w-2xl w-full max-h-[90vh] overflow-y-auto">
      <div class="p-6">
        <div class="flex justify-between items-center mb-4">
          <h3 class="text-lg font-semibold">订单详情</h3>
          <button @click="closeDetailModal" class="text-slate-500 hover:text-slate-700">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
        
        <div v-if="selectedOrder" class="space-y-6">
          <div class="flex flex-col md:flex-row md:items-start gap-6">
            <div class="flex-shrink-0">
              <img class="h-24 w-24 object-contain" :src="selectedOrder.productImage" :alt="selectedOrder.productName" />
            </div>
            <div class="flex-1">
              <h4 class="text-lg font-medium text-slate-900">{{ selectedOrder.productName }}</h4>
              <div class="mt-1 text-sm text-slate-500">SKU: {{ selectedOrder.productSku }}</div>
              <div class="mt-2 flex flex-wrap gap-2">
                <span class="px-2 py-1 text-xs font-medium rounded-full" :class="getOrderTypeClass(selectedOrder.orderType)">
                  {{ selectedOrder.orderType === ' wholesale' ? '批发' : '一件代发' }}
                </span>
                <span class="px-2 py-1 text-xs font-medium rounded-full" :class="getStatusClass(selectedOrder.status)">
                  {{ selectedOrder.status }}
                </span>
              </div>
            </div>
            <div class="text-right">
              <div class="text-2xl font-bold text-slate-900">${{ selectedOrder.totalAmount }}</div>
              <div class="text-sm text-slate-500 mt-1">{{ selectedOrder.quantity }} 件 × ${{ selectedOrder.unitPrice }}</div>
            </div>
          </div>
          
          <div class="border-t border-slate-200 pt-4">
            <h5 class="text-md font-medium text-slate-900 mb-3">订单信息</h5>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <label class="text-sm font-medium text-slate-700">订单号</label>
                <div class="mt-1 text-sm text-slate-900">{{ selectedOrder.orderNumber }}</div>
              </div>
              <div>
                <label class="text-sm font-medium text-slate-700">下单时间</label>
                <div class="mt-1 text-sm text-slate-900">{{ selectedOrder.orderDate }}</div>
              </div>
              <div class="md:col-span-2">
                <label class="text-sm font-medium text-slate-700">收货地址</label>
                <div class="mt-1 text-sm text-slate-900">{{ selectedOrder.deliveryAddress }}</div>
              </div>
              <div class="md:col-span-2">
                <label class="text-sm font-medium text-slate-700">备注</label>
                <div class="mt-1 text-sm text-slate-900">{{ selectedOrder.remarks || '无' }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface OrderItem {
  id: number
  productImage: string
  productName: string
  productSku: string
  unitPrice: number
  quantity: number
  totalAmount: number
  status: string
  orderDate: string
  orderNumber: string
  deliveryAddress: string
  orderType: ' wholesale' | 'dropshipping' // wholesale: 批发, dropshipping: 一件代发
  remarks?: string
}

const currentPage = ref(1)
const pageSize = ref(5)
const showDetailModal = ref(false)
const selectedOrder = ref<OrderItem | null>(null)
const activeFilter = ref<'all' | ' wholesale' | 'dropshipping'>('all')
const activeStatusFilter = ref<'all' | '待付款' | '待发货' | '已发货' | '已完成' | '已取消'>('all')
const searchKeyword = ref('')

// 演示数据
const orders: OrderItem[] = [
  {
    id: 1,
    productImage: 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202312/b4a7be3d-a601-4332-a34d-47833226c810.Jpeg',
    productName: '3抽抽屉柜 床头柜储物柜 白色（同款07479869, 69269387）',
    productSku: '75682614',
    unitPrice: 46.80,
    quantity: 2,
    totalAmount: 93.60,
    status: '待发货',
    orderDate: '2025-10-15 14:30:25',
    orderNumber: 'ORD20251015001',
    deliveryAddress: '北京市朝阳区xxx街道xxx号',
    orderType: ' wholesale',
    remarks: '请尽快发货'
  },
  {
    id: 2,
    productImage: 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202308/4695cd17-10c7-473c-960a-fbb9d18c4a90.Jpeg',
    productName: '12ft 巨型可怕幽灵 4颗LED灯 火焰和闪烁的红眼睛 充气万圣装饰',
    productSku: '50904039',
    unitPrice: 34.04,
    quantity: 1,
    totalAmount: 34.04,
    status: '已发货',
    orderDate: '2025-10-14 09:15:42',
    orderNumber: 'ORD20251014002',
    deliveryAddress: '上海市浦东新区xxx路xxx号',
    orderType: 'dropshipping',
    remarks: '物流单号：FEDEX123456789'
  },
  {
    id: 3,
    productImage: 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202309/3c1b039b-de57-4d8f-805a-4d15658b90c5.Jpeg',
    productName: '6ft LED南瓜灯 万圣节充气装饰',
    productSku: '50904040',
    unitPrice: 22.50,
    quantity: 3,
    totalAmount: 67.50,
    status: '已完成',
    orderDate: '2025-10-10 16:45:18',
    orderNumber: 'ORD20251010003',
    deliveryAddress: '广州市天河区xxx大道xxx号',
    orderType: ' wholesale',
    remarks: '客户已签收'
  },
  {
    id: 4,
    productImage: 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202312/b4a7be3d-a601-4332-a34d-47833226c810.Jpeg',
    productName: '现代简约3抽屉梳妆台 白色',
    productSku: '75682615',
    unitPrice: 52.00,
    quantity: 1,
    totalAmount: 52.00,
    status: '待付款',
    orderDate: '2025-10-08 11:20:33',
    orderNumber: 'ORD20251008004',
    deliveryAddress: '深圳市南山区xxx科技园xxx栋',
    orderType: 'dropshipping',
    remarks: ''
  },
  {
    id: 5,
    productImage: 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202308/03b9a883-ada3-41af-88a5-0beba02f2eeb.Jpeg',
    productName: '万圣节幽灵装饰灯 黑色款',
    productSku: '50904041',
    unitPrice: 28.90,
    quantity: 2,
    totalAmount: 57.80,
    status: '已取消',
    orderDate: '2025-10-05 13:45:12',
    orderNumber: 'ORD20251005005',
    deliveryAddress: '杭州市西湖区xxx路xxx号',
    orderType: ' wholesale',
    remarks: '客户主动取消'
  },
  {
    id: 6,
    productImage: 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202312/86bb50ad-7a28-4ade-982f-600530c9bb3f.Jpeg',
    productName: '北欧风格床头柜 实木材质',
    productSku: '75682616',
    unitPrice: 65.50,
    quantity: 1,
    totalAmount: 65.50,
    status: '待发货',
    orderDate: '2025-10-01 10:30:45',
    orderNumber: 'ORD20251001006',
    deliveryAddress: '成都市锦江区xxx街道xxx号',
    orderType: 'dropshipping',
    remarks: '样品确认通过'
  },
  {
    id: 7,
    productImage: 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202309/2a764166-da43-45a1-9fba-ceea6243b6b7.Jpeg',
    productName: '户外庭院装饰灯 太阳能款',
    productSku: '50904042',
    unitPrice: 18.75,
    quantity: 5,
    totalAmount: 93.75,
    status: '已发货',
    orderDate: '2025-09-28 15:22:17',
    orderNumber: 'ORD20250928007',
    deliveryAddress: '武汉市江汉区xxx大道xxx号',
    orderType: ' wholesale',
    remarks: '批量采购订单'
  },
  {
    id: 8,
    productImage: 'https://img-accelerate.saleyee.cn/Resources/GoodsImages/2023/202312/2753d850-5827-4fcb-a4ac-bea08144b53e.Jpeg',
    productName: '儿童房储物柜 粉色系列',
    productSku: '75682617',
    unitPrice: 39.90,
    quantity: 1,
    totalAmount: 39.90,
    status: '已完成',
    orderDate: '2025-09-25 09:45:30',
    orderNumber: 'ORD20250925008',
    deliveryAddress: '南京市鼓楼区xxx路xxx号',
    orderType: 'dropshipping',
    remarks: '节日促销订单'
  }
]

const filteredOrders = computed(() => {
  let result = orders
  
  // 按订单类型过滤
  if (activeFilter.value !== 'all') {
    result = result.filter(order => order.orderType === activeFilter.value)
  }
  
  // 按订单状态过滤
  if (activeStatusFilter.value !== 'all') {
    result = result.filter(order => order.status === activeStatusFilter.value)
  }
  
  // 按关键词搜索
  if (searchKeyword.value) {
    const keyword = searchKeyword.value.toLowerCase()
    result = result.filter(order => 
      order.productName.toLowerCase().includes(keyword) || 
      order.productSku.toLowerCase().includes(keyword) || 
      order.orderNumber.toLowerCase().includes(keyword)
    )
  }
  
  return result
})

const totalOrders = computed(() => filteredOrders.value.length)
const totalPages = computed(() => Math.ceil(totalOrders.value / pageSize.value))

const paginatedOrders = computed(() => {
  const start = (currentPage.value - 1) * pageSize.value
  const end = start + pageSize.value
  return filteredOrders.value.slice(start, end)
})

const paginationRange = computed(() => {
  const delta = 2
  const left = Math.max(1, currentPage.value - delta)
  const right = Math.min(totalPages.value, currentPage.value + delta)
  const range = []

  if (left > 1) {
    range.push(1)
    if (left > 2) {
      range.push('...')
    }
  }

  for (let i = left; i <= right; i++) {
    range.push(i)
  }

  if (right < totalPages.value) {
    if (right < totalPages.value - 1) {
      range.push('...')
    }
    range.push(totalPages.value)
  }

  return range
})

const goToPage = (page: number | string) => {
  if (typeof page === 'number') {
    currentPage.value = page
  }
}

const previousPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
  }
}

const viewDetail = (id: number) => {
  selectedOrder.value = orders.find(item => item.id === id) || null
  showDetailModal.value = true
}

const closeDetailModal = () => {
  showDetailModal.value = false
  selectedOrder.value = null
}

const filterOrders = (filter: 'all' | ' wholesale' | 'dropshipping') => {
  activeFilter.value = filter
  currentPage.value = 1 // 重置到第一页
}

const filterByStatus = (status: 'all' | '待付款' | '待发货' | '已发货' | '已完成' | '已取消') => {
  activeStatusFilter.value = status
  currentPage.value = 1 // 重置到第一页
}

const searchOrders = () => {
  currentPage.value = 1 // 重置到第一页
  // 搜索功能将在过滤逻辑中实现
}

const resetFilters = () => {
  activeFilter.value = 'all'
  activeStatusFilter.value = 'all'
  searchKeyword.value = ''
  currentPage.value = 1
}

const getStatusClass = (status: string) => {
  switch (status) {
    case '待付款':
      return 'bg-yellow-100 text-yellow-800'
    case '待发货':
      return 'bg-blue-100 text-blue-800'
    case '已发货':
      return 'bg-indigo-100 text-indigo-800'
    case '已完成':
      return 'bg-green-100 text-green-800'
    case '已取消':
      return 'bg-red-100 text-red-800'
    default:
      return 'bg-gray-100 text-gray-800'
  }
}

const getOrderTypeClass = (orderType: ' wholesale' | 'dropshipping') => {
  switch (orderType) {
    case ' wholesale':
      return 'bg-purple-100 text-purple-800'
    case 'dropshipping':
      return 'bg-cyan-100 text-cyan-800'
    default:
      return 'bg-gray-100 text-gray-800'
  }
}
</script>

<style scoped>
/* 可以根据需要添加自定义样式 */
</style>