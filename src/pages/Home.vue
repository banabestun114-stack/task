<!--
  Home Page Component
  Main landing page with responsive design for mobile and desktop
-->
<template>
  <div class="home-page">

    <!--mobile header -->
    <v-container fluid class="pa-0">
      <v-sheet class="header" height="166px" v-if="isMobile">
        <v-img cover height="166px"
          src="https://images.unsplash.com/photo-1521334884684-d80222895322?q=80&w=1920&auto=format&fit=crop"
          gradient="to bottom, rgba(255,255,255,0), rgba(50,200,154,4.80)">
          <!-- Content layered on top of the image -->
          <div class="overlay-content">
            <!-- Search + Cart -->
            <div class="search-row">
              <v-text-field v-model="searchQuery" variant="solo" bg-color="white" hide-details density="comfortable"
                rounded="xl" class="search-input flex-grow-1" prepend-inner-icon="mdi-magnify"
                placeholder="Search Item or Brand .." @input="debouncedSearch"
                :append-inner-icon="searchQuery ? 'mdi-close' : undefined" @click:append-inner="clearSearch" />
              <v-sheet class="cart glass rounded-lg d-flex align-center justify-center" @click="goToCart">
                <v-icon size="28">mdi-cart-outline</v-icon>
              </v-sheet>
            </div>

            <!-- Horizontal filter chips -->
            <v-container v-if="isMobile" class="py-4">
              <v-chip-group v-model="selectedCategoryIndex" mandatory>
                <v-chip v-for="category in displayCategories" :key="category.value" :value="category.value"
                  color="primary" variant="outlined" class="blur-card" @click="selectCategory(category.value)">
                  {{ category.label }}
                </v-chip>
              </v-chip-group>
            </v-container>
          </div>
        </v-img>
      </v-sheet>
    </v-container>

    <!-- Mobile Featured Slider -->
    <v-container v-if="isMobile" class="mobile-featured-slider mt-2 pa-3">
      <div class="featured-scroll no-scrollbar">
        <div class="d-flex flex-row" style="gap:50px;">

          <div v-for="(product, index) in featuredProducts" :key="product.id" class="featured-card-mobile">
            <v-card :color="getSlideColor(index)" class="d-flex align-center pa-3 rounded-xl" height="170" width="300">
              <div class="d-flex flex-row align-center" style="width:100%;">
                <div class="text-white" style="flex:1;">
                  <h3 class="text-Chiller-1  font-weight-medium mb-1 ">American Style</h3>
                  <p class=" text-caption font-weight-medium mb-1">Crossbody Bucked Bag</p>
                  <h2>IQD {{ Math.round(product.price * 40) }}</h2>
                  <p class="text-caption">Sizes M &nbsp; L &nbsp; XL</p>
                </div>
                <div style="margin-left:12px;">
                  <v-img :src="product.image" height="90" width="90" contain style="border-radius:15px;" />
                </div>
              </div>
            </v-card>
          </div>
        </div>
      </div>
    </v-container>

    <!-- Mobile Categories Section -->
    <v-container v-if="isMobile">
      <div class="d-flex justify-space-between align-center mb-4">
        <h3>Our Categories</h3>
        <p color="gray" size="10px" @click="$router.push('/categories')">
          Show All
        </p>
      </div>
      <div class="mobile-category-scroll no-scrollbar">

        <div class="d-flex flex-row" style="gap:10px;">
          <div v-for="category in categories.slice(0, 8)" :key="category"
            class="category-card-mobile d-flex align-center" @click="selectCategory(category)">
            <v-icon size="28" class="mr-2">
              {{ getCategoryIcon(category) }}
            </v-icon>
            <span class="category-label-mobile text-capitalize">{{ category.replace('-', ' & ') }}</span>
          </div>
        </div>
      </div>
    </v-container>

    <!-- Desktop Content -->
    <div v-if="!isMobile" class="desktop-content">
      <!-- Featured Slider Section -->
      <v-container class="py-6">

        <div class="featured-scroll no-scrollbar d-flex" style="gap:16px;">
          <div class="d-flex flex-row" style="gap:15px;">
            <v-card v-for="(product, index) in featuredProducts" :key="product.id" :color="getSlideColor(index)"
              class="d-flex align-center rounded-xl pa-4" height="177.34" width="357.16" rounded-lg>
              <div class="text-white" style="flex:1;">
                <h3 class="text-Chiller-1  font-weight-medium mb-1">American Style</h3>
                <!-- <v-divider vertical class="mx-3" /> -->
                <h4 class="text-caption font-weight-medium mb-1">{{ product.title.substring(0, 30) }}...</h4>
                <h2>IQD {{ Math.round(product.price * 40) }}</h2>
                <p class="text-caption">Sizes: M &nbsp; L &nbsp; XL</p>
              </div>
              <v-img :src="product.image" height="120" contain />
            </v-card>
          </div>
        </div>

        <!-- Desktop Filters and Search -->
        <v-row class="mb-4" dense>
          <v-col cols="12">
            <v-card class="pa-2" flat tile>
              <v-row align="center" dense class="gap-x-2">

                <!-- Search -->
                <v-col cols="3">
                  <v-text-field v-model="searchQuery" placeholder="Search Item" prepend-inner-icon="mdi-magnify"
                    variant="outlined" density="compact" hide-details @input="handleSearch" />
                </v-col>

                <!-- Category Filter -->
                <v-col cols="auto">
                  <v-select v-model="selectedCategory" :items="categoryOptions" variant="outlined" density="compact"
                    hide-details @update:model-value="selectCategory" />
                </v-col>

                <!-- Brand Filter -->
                <v-col cols="1">
                  <v-select :items="brandOptions" label="Brand" variant="outlined" density="compact" hide-details />
                </v-col>

                <!-- Price Range -->
                <v-col cols="1">
                  <v-select :items="priceRangeOptions" label="Price Range" variant="outlined" density="compact"
                    hide-details />
                </v-col>

                <!-- Express -->
                <v-col cols="auto">
                  <div class="d-flex align-center">
                    <span class="mr-">&nbsp;&nbsp;&nbsp;&nbsp; </span>
                    <v-icon class="mr-2" color="amber darken-2">mdi-truck-delivery</v-icon>
                    <span class="mr-">Express&nbsp;</span>
                    <v-switch v-model="express" inset color="teal" hide-details />
                    <v-divider vertical class="mx-3" />
                  </div>
                </v-col>

                <!-- Sort -->
                <v-col cols="auto">
                  <v-select v-model="sortBy" :items="sortOptions" hide-details variant="plain" density="compact"
                    class="sort-inline" @update:model-value="handleSort">
                    <template v-slot:selection="{ item }">
                      <span class="sort-label">Sort by</span>
                      <span class="sort-value">{{ item.title }}</span>
                    </template>
                  </v-select>
                </v-col>

                <!-- Settings -->
                <v-col cols="auto" class="text-center">
                  <v-icon>mdi-cog</v-icon>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </div>

    <!-- Products Grid -->
    <v-container>
      <v-row>
        <v-col v-for="product in filteredProducts" :key="product.id" :cols="isMobile
          ? 6 : 3" :sm="isMobile
            ? 6 : 4" :md="isMobile
              ? 4 : 3" :lg="isMobile
              ? 3 : 3">

          <ProductCard :product="product" :is-desktop="!isMobile" />
        </v-col>
      </v-row>
      <div v-if="loading" class="text-center py-8">
        <v-progress-circular indeterminate color="primary" />
      </div>
    </v-container>

    <!-- Mobile Cart Items Section -->
    <v-container v-if="isMobile" class="cart-items-section">
      <v-container class="d-flex justify-center align-center">
        <v-btn class="beautiful-btn" size="x-large" elevation="12">
          Show All Products
        </v-btn>
      </v-container>
      <div class="d-flex justify-space-between align-center mb-4">
        <h3>Recentely Added</h3>
        <p color="gray" size="10px"> <!--@click="$router.push('/cart')"-->
          View All
        </p>
      </div>
      <div class="cart-items-grid">
        <div v-for="(item, index) in cartItems" :key="index" class="cart-item-card">
          <v-card class="cart-card" elevation="0">
            <div class="cart-image-container">
              <v-img :src="item.image" height="100" cover class="cart-image" />
              <div class="cart-brand-chip">
                <span class="cart-brand-text">{{ item.brand }}</span>
              </div>
              <v-btn icon size="small" class="cart-favorite-btn" variant="text">
                <v-icon size="18">mdi-heart-outline</v-icon>
              </v-btn>
            </div>
            <v-card-text class="cart-details pa-3">
              <h4 class="cart-title mb-2">{{ item.title }}</h4>
              <div class="cart-seller-info d-flex align-center mb-2">
                <v-avatar size="18" class="mr-2">
                  <v-img src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg" />
                </v-avatar>
                <span class="cart-seller-name">{{ item.seller }}</span>
              </div>
              <div class="cart-price-size-row d-flex justify-space-between align-center">
                <span class="cart-price">{{ item.price }}</span>
                <span class="cart-size">{{ item.size }}</span>
              </div>
            </v-card-text>
          </v-card>
        </div>
      </div>
    </v-container>
  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue'
import { useDisplay } from 'vuetify'
import { useRouter } from 'vue-router'
import { useProductStore } from '../stores/products'
import { useCartStore } from '../stores/cart'
import ProductCard from '../components/ProductCard.vue'

export default {
  name: 'HomePage',
  components: {
    ProductCard,
  },
  setup() {
    const { mobile } = useDisplay()
    const router = useRouter()
    const productStore = useProductStore()
    const cartStore = useCartStore()

    const searchQuery = ref('')
    const selectedCategoryIndex = ref(0)
    const selectedCategory = ref('all')
    const sortBy = ref('popular')
    const express = ref(true)
    const searchTimeout = ref(null)

    // Sample cart items data
    const cartItems = ref([
      {
        image: 'https://images.pexels.com/photos/2534961/pexels-photo-2534961.jpeg',
        brand: 'Zara',
        title: 'Crossbody Bucket Bag',
        seller: 'Khalid S.',
        price: 'IQD 983',
        size: 'Size M'
      },
      {
        image: 'https://images.pexels.com/photos/1647976/pexels-photo-1647976.jpeg',
        brand: 'Zara',
        title: 'Leather Crossbody',
        seller: 'Khalid S.',
        price: 'IQD 1,250',
        size: 'Size L'
      },
      {
        image: 'https://images.pexels.com/photos/2534961/pexels-photo-2534961.jpeg',
        brand: 'Zara',
        title: 'Mini Shoulder Bag',
        seller: 'Khalid S.',
        price: 'IQD 750',
        size: 'Size S'
      },
      {
        image: 'https://images.pexels.com/photos/1647976/pexels-photo-1647976.jpeg',
        brand: 'Zara',
        title: 'Canvas Tote Bag',
        seller: 'Khalid S.',
        price: 'IQD 890',
        size: 'Size M'
      }
    ])

    // Responsive detection
    const isMobile = computed(() => mobile.value)

    // Category options for mobile
    const displayCategories = computed(() => [
      { label: 'Popular', value: 'all' },
      { label: 'Electronics', value: 'electronics' },
      { label: 'Jewelery', value: 'jewelery' },
      { label: 'Men\'s Clothing', value: "men's clothing" },
      { label: 'Women\'s Clothing', value: "women's clothing" },
    ])

    // Desktop filter options
    const categoryOptions = computed(() => [
      { title: 'All Categories', value: 'all' },
      { title: 'Electronics', value: 'electronics' },
      { title: 'Jewelery', value: 'jewelery' },
      { title: 'Men\'s Clothing', value: "men's clothing" },
      { title: 'Women\'s Clothing', value: "women's clothing" },
    ])


    const priceRangeOptions = [
      { title: 'All Prices', value: 'all' },
      { title: 'Under 500 IQD', value: '0-500' },
      { title: '500-1000 IQD', value: '500-1000' },
      { title: 'Over 1000 IQD', value: '1000+' },
    ]

    const sortOptions = [
      { title: 'Popular', value: 'popular' },
      { title: 'Price: Low to High', value: 'price-low' },
      { title: 'Price: High to Low', value: 'price-high' },
      { title: 'Newest', value: 'newest' },
      { title: 'Rating', value: 'rating' },
    ]

    // Store getters
    const featuredProducts = computed(() => productStore.featuredProducts)
    const categories = computed(() => productStore.categories)
    const filteredProducts = computed(() => productStore.filteredProducts)
    const loading = computed(() => productStore.loading)

    /**
     * Handle search input with debouncing for better performance
     */
    const debouncedSearch = () => {
      // Clear existing timeout
      if (searchTimeout.value) {
        clearTimeout(searchTimeout.value)
      }

      // Set new timeout for 300ms delay
      searchTimeout.value = setTimeout(() => {
        productStore.setSearchQuery(searchQuery.value)
      }, 300)
    }

    /**
     * Handle search input (for desktop)
     */
    const handleSearch = () => {
      productStore.setSearchQuery(searchQuery.value)
    }

    /**
     * Select category filter
     * @param {string} category - Category to select
     */
    const selectCategory = (category) => {
      selectedCategory.value = category
      productStore.setSelectedCategory(category)
    }

    /**
     * Handle sort option change
     * @param {string} sortOption - Sort option to apply
     */
    const handleSort = (sortOption) => {
      productStore.setSortBy(sortOption)
    }

    /**
     * Get category icon
     * @param {string} category - Category name
     * @returns {string} Icon name
     */
    const getCategoryIcon = (category) => {
      const icons = {
        electronics: 'mdi-laptop',
        jewelery: 'mdi-diamond-stone',
        "men's clothing": 'mdi-tshirt-crew',
        "women's clothing": 'mdi-dress',
      }
      return icons[category] || 'mdi-tag'
    }

    /**
     * Get slide color for carousel
     * @param {number} index - Slide index
     * @returns {string} Color name
     */
    const getSlideColor = (index) => {
      const colors = ['#ff7043', '#616161', '#26a69a', '#ab47bc']
      return colors[index % colors.length]
    }

    /**
     * Navigate to cart page
     */
    const goToCart = () => {
      router.push('/cart')
    }

    /**
     * Clear the search query
     */
    const clearSearch = () => {
      searchQuery.value = ''
      productStore.setSearchQuery('')
    }

    // Initialize data on component mount
    onMounted(async () => {
      await productStore.fetchProducts()
      await productStore.fetchCategories()
    })
    return {
      // Reactive data
      searchQuery,
      selectedCategoryIndex,
      selectedCategory,
      sortBy,
      cartItems,
      // Computed
      isMobile,
      displayCategories,
      categoryOptions,
      priceRangeOptions,
      sortOptions,
      featuredProducts,
      categories,
      filteredProducts,
      loading,
      // Stores
      cartStore,
      // Methods
      debouncedSearch,
      handleSearch,
      selectCategory,
      handleSort,
      getCategoryIcon,
      getSlideColor,
      goToCart,
      clearSearch,
    }
  },
}
</script>

<style scoped>
.home-page {
  min-height: 1630px;
  background-color: white;
}

.search-field {
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 25px;
}

.category-card {
  cursor: pointer;
  transition: all 0.2s;
}

.category-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.featured-carousel {
  border-radius: 16px;
  overflow: hidden;
}

.desktop-content {
  /* padding-top: 5px; */
  padding-bottom: 0px;
  padding-top: 0px;
  margin-top: 0px;
  /* Account for fixed header */
}

.row {
  background-color: transparent !important;
  border: none !important;
  box-shadow: none !important;
}

/* Mobile specific styles */
@media (max-width: 375.7px) {
  .desktop-content {
    padding-top: 0px;
  }
}

.sort-inline {
  min-width: 160px;
  border: none;
  box-shadow: none;
}

.sort-inline .sort-label {
  font-size: 14px;
  color: #817474;
  margin-right: 4px;
}

.sort-inline .sort-value {
  font-weight: 600;
  /* make Popular bold */
}

.sort-inline .v-select__selection-text {
  display: flex;
  align-items: center;
}

.sort-inline .v-field {
  border: none !important;
  box-shadow: none !important;
  background: transparent !important;
}

.mobile-category-scroll {
  overflow-x: auto;
  padding-bottom: 2px;
  margin-bottom: 3px;
}

.no-scrollbar {
  scrollbar-width: none;
  /* Firefox */
  -ms-overflow-style: none;
  /* IE 10+ */
}

.no-scrollbar::-webkit-scrollbar {
  display: none;
  /* Chrome, Safari, Opera */
}

.category-card-mobile {
  min-width: 180px;
  max-width: 250px;
  height: 44px;
  background: #fff;
  border: 1px solid #eee;
  border-radius: 14px;
  box-shadow: 0 2px 8px rgba(38, 166, 154, 0.06);
  padding: 0 16px;
  margin-right: 4px;
  cursor: pointer;
  transition: box-shadow 0.20s, background 0.18s;
  display: flex;
  align-items: center;
  white-space: nowrap;
}


.category-label-mobile {
  font-size: 12px;
  font-weight: 500;
  margin-left: 4px;
  letter-spacing: 0.01em;
  color: #222;
}


.mobile-featured-slider {
  padding-bottom: 0px;
  padding-top: 0px;
  margin-top: 0px;
}

.featured-scroll {
  overflow-x: auto;
  padding-bottom: 4px;
  padding-top: 0px;
  margin-bottom: 8px;
}

.no-scrollbar {
  scrollbar-width: none;
  /* Firefox */
  -ms-overflow-style: none;
  /* IE 10+ */
}

.no-scrollbar::-webkit-scrollbar {
  display: none;
  /* Chrome, Safari, Opera */
}

.featured-card-mobile {
  min-width: 240px;
  max-width: 260px;
  flex: 0 0 auto;
}

.header {
  position: relative;
}

.rounded-b-xl {
  border-bottom-left-radius: 16px;
  border-bottom-right-radius: 16px;
}

/* Layout of the overlayed content */
.overlay-content {
  position: absolute;
  inset: 0;
  padding: 16px 18px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

/* Search row (search + cart) */
.search-row {
  display: flex;
  gap: 12px;
  margin-top: 19px;
  width: 100%;
  height: 300px;
}

/* Make the text-field look like a floating pill */
.search-input :deep(.v-field) {
  border-radius: 15px !important;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
}

.search-input :deep(input::placeholder) {
  opacity: 0.9;
}

/* Glassmorphism cart button */
.cart {
  width: 56px;
  height: 56px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.cart:hover {
  transform: scale(1.05);
}

.glass {
  background: none;
  border: 1px solid rgb(255, 255, 255);
}

.blur-card {
  background: rgba(20, 32, 4, 0.2);
  color: #eee;
  /* semi-transparent background */
  border-radius: 16px;
  border: none;
  /* subtle border */
  backdrop-filter: blur(12px);
  /* MAIN blur effect */
  -webkit-backdrop-filter: blur(12px);
  /* Safari support */
}

/* Recently Added Section Styles */
.recently-added-section {
  padding: 0 16px;
  margin-bottom: 24px;
}

.recently-added-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.recent-product-card {
  width: 100%;
}

.product-card {
  border-radius: 12px;
  overflow: hidden;
}

.image-container {
  position: relative;
}

.product-image {
  width: 100%;
}

.brand-chip {
  position: absolute;
  top: 8px;
  left: 8px;
  background: white;
  border-radius: 12px;
  padding: 4px 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.brand-text {
  font-size: 10px;
  font-weight: 600;
  color: #000;
}

.favorite-btn {
  position: absolute;
  top: 8px;
  right: 8px;
  background: white;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.image-dots {
  position: absolute;
  bottom: 8px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 4px;
}

.dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.6);
}

.dot.active {
  background: white;
}

.product-title {
  font-size: 14px;
  font-weight: 600;
  color: #000;
  margin: 0;
}

.seller-name {
  font-size: 12px;
  color: #666;
}

.price {
  font-size: 14px;
  font-weight: 700;
  color: #000;
}

.size {
  font-size: 12px;
  color: #999;
}

/* Cart Items Section Styles */
.cart-items-section {
  padding: 0 16px;
  margin-bottom: 24px;
}

.cart-items-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.cart-item-card {
  width: 100%;
}

.cart-card {
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 15px;
  overflow: hidden;
  height: 100%;
  background-color: transparent !important;
  border: none !important;
  box-shadow: none !important;
}

.cart-card:hover {
  transform: translateY(1px);
  box-shadow: 0;
}

.cart-image-container {
  position: relative;
}

.cart-image {
  width: 100%;
  transition: transform 0.3s ease;
}

.cart-card:hover .cart-image {
  transform: scale(1.05);
}

.cart-brand-chip {
  position: absolute;
  top: 10px;
  left: 8px;
  background: rgb(231, 228, 228);
  border-radius: 14px;
  padding: 4px 8px;
  /* box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); */
  width: 39px;
  /* height: 25px; */
}

.cart-brand-text {
  font-size: 10px;
  font-weight: 600;
  color: #000;
}

.cart-favorite-btn {
  position: absolute;
  top: 8px;
  right: 8px;
  background: white;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.cart-title {
  font-size: 13px;
  font-weight: 600;
  color: #000;
  margin: 0;
}

.cart-seller-name {
  font-size: 11px;
  color: #666;
}

.cart-price {
  font-size: 13px;
  font-weight: 700;
  color: #000;
}

.cart-size {
  font-size: 11px;
  color: #999;
}

.beautiful-btn {
  background: none !important;
  border-width: 0.5px !important;
  /* ultra thin border */
  font-weight: 300;
  font-size: 13px;
  letter-spacing: 1px;
  border-radius: 12px;
  box-shadow: none !important;
  width: 400px;
  height: 50px;
}
</style>