<!--
  Home Page Component
  Main landing page with responsive design for mobile and desktop
-->
<template>
  <div class="home-page">
    <!-- Mobile Header -->
    <!-- <v-app-bar
    color="primary"
    flat
    height="150"
    class="flex-column"
    v-if="isMobile"
  > -->

    <v-container fluid class="pa-0">
      <v-sheet class="header" height="240" v-if="isMobile">
        <v-img cover height="170"
          src="https://images.unsplash.com/photo-1521334884684-d80222895322?q=80&w=1920&auto=format&fit=crop"
          gradient="to bottom, rgba(255,255,255,0), rgba(50,200,154,0.80)">
          <!-- Content layered on top of the image -->
          <div class="overlay-content">
            <!-- Search + Cart -->
            <div class="search-row">
              <v-text-field v-model="q" variant="solo" bg-color="white" hide-details density="comfortable" rounded="xl"
                class="search-input flex-grow-1" prepend-inner-icon="mdi-magnify"
                placeholder="Search Item or Brand .." />
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
    <!-- </v-app-bar> -->




    <!-- Desktop Content -->
    <div v-if="!isMobile" class="desktop-content">
      <!-- Featured Slider Section -->
      <v-container class="py-6">
        <div class="featured-scroll no-scrollbar d-flex" style="gap:16px;">
          <v-card v-for="(product, index) in featuredProducts" :key="product.id" :color="getSlideColor(index)"
            class="d-flex pa-4" height="200" width="380" rounded-lg>

            <div class="text-white text-center mt-3">
              <h3 class="text-h6 mb-1">American Style</h3>
              <h4 class="text-subtitle-2 mb-1">{{ product.title.substring(0, 30) }}...</h4>
              <h2 class="text-subtitle-1 mb-1">IQD {{ Math.round(product.price * 25) }}</h2>
              <p class="text-caption">Sizes: M L XL</p>
            </div>
            <v-img :src="product.image" height="120" contain />
          </v-card>
        </div>

        <!-- Desktop Filters and Search -->
        <v-row class="mb-6">
          <v-col cols="12">
            <v-card class="pa-4">
              <v-row align="center">

                <!-- Search -->
                <v-col cols="2">
                  <v-text-field v-model="searchQuery" placeholder="Search Item " prepend-inner-icon="mdi-magnify"
                    variant="outlined" density="compact" hide-details @input="handleSearch" />
                </v-col>

                <!-- Category Filter -->
                <v-col cols="2">
                  <v-select v-model="selectedCategory" :items="categoryOptions" label="Category" variant="outlined"
                    density="compact" hide-details @update:model-value="selectCategory" />
                </v-col>

                <!-- Brand Filter -->
                <v-col cols="2">
                  <v-select :items="brandOptions" label="Brand" variant="outlined" density="compact" hide-details />
                </v-col>

                <!-- Price Range -->
                <v-col cols="2">
                  <v-select :items="priceRangeOptions" label="Price Range" variant="outlined" density="compact"
                    hide-details />

                </v-col>

                <v-col cols="1">
                  <div class="d-flex align-center">
                    <v-icon class="mr-2" color="amber darken-2">mdi-truck-delivery</v-icon>

                    <!-- <span class="mr-">Express</span> -->
                    <v-switch v-model="express" inset color="teal" hide-details></v-switch>
                  </div>
                </v-col>

                <!-- Sort -->
                <v-col cols="2">
                  <v-select v-model="sortBy" :items="sortOptions" hide-details variant="plain" density="compact"
                    class="sort-inline" @update:model-value="handleSort">
                    <template v-slot:selection="{ item }">

                      <span class="sort-label">Sort by</span>
                      <span class="sort-value">{{ item.title }}</span>
                    </template>
                  </v-select>
                </v-col>

                <v-col cols="1" class="text-center">
                  <v-icon>mdi-cog</v-icon>
                </v-col>

              </v-row>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </div>

    <!-- Mobile Featured Slider -->
    <v-container v-if="isMobile" class="mobile-featured-slider">
      <div class="featured-scroll no-scrollbar">
        <div class="d-flex flex-row" style="gap:50px;">

          <div v-for="product in featuredProducts" :key="product.id" class="featured-card-mobile">
            <v-card color="secondary" class="d-flex align-center pa-4 rounded-xl" height="170" width="300">
              <div class="d-flex flex-row align-center" style="width:100%;">
                <div class="text-white" style="flex:1;">
                  <h3 class="text-subtitle-1 font-weight-medium mb-1 ">American Style</h3>
                  <p class=" text-caption font-weight-medium mb-1">Crossbody Bucked Bag</p>
                  <h2>IQD {{ Math.round(product.price * 25) }}</h2>
                  <p class="text-caption">Sizes M L XL</p>
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

    <!-- Products Grid -->
    <v-container>
      <v-row>
        <v-col v-for="product in filteredProducts" :key="product.id" :cols="isMobile ? 6 : 3" :sm="isMobile ? 6 : 4"
          :md="isMobile ? 4 : 3" :lg="isMobile ? 3 : 3">
          <ProductCard :product="product" :is-desktop="!isMobile" />
        </v-col>
      </v-row>

      <div v-if="loading" class="text-center py-8">
        <v-progress-circular indeterminate color="primary" />
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
    const express = ref(true);

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

    const brandOptions = [
      { title: 'All Brands', value: 'all' },
      { title: 'Zara', value: 'zara' },
      { title: 'H&M', value: 'hm' },
      { title: 'Apple', value: 'apple' },
    ]

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
     * Handle search input
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

      // Computed
      isMobile,
      displayCategories,
      categoryOptions,
      brandOptions,
      priceRangeOptions,
      sortOptions,
      featuredProducts,
      categories,
      filteredProducts,
      loading,

      // Stores
      cartStore,

      // Methods
      handleSearch,
      selectCategory,
      handleSort,
      getCategoryIcon,
      getSlideColor,
      goToCart,
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
  padding-top: 5px;
  /* Account for fixed header */
}

.row {
  background-color: transparent !important;
  border: none !important;
  box-shadow: none !important;
}

/* Mobile specific styles */
@media (max-width: 375px) {
  .desktop-content {
    padding-top: 0px;
  }
}

.sort-inline {
  min-width: 160px;
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


.mobile-category-scroll {
  overflow-x: auto;
  padding-bottom: 4px;
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

.category-card-mobile {
  min-width: 170px;
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
  padding: 16px 20px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

/* Search row (search + cart) */
.search-row {
  display: flex;
  gap: 12px;
  margin-top: 19px;
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
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.2);
}

.glass {
  background: rgba(255, 255, 255, 0.28);
  border: 1px solid rgba(255, 255, 255, 0.55);
  backdrop-filter: blur(8px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
}

.blur-card {

  background: rgba(20, 32, 4, 0.2);
  /* semi-transparent background */
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.4);
  /* subtle border */
  backdrop-filter: blur(12px);
  /* MAIN blur effect */
  -webkit-backdrop-filter: blur(12px);
  /* Safari support */
}
</style>