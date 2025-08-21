<!--
  Product Detail Page Component mobile and desctop
  Displays detailed product information with responsive design
-->
<template>
  <div v-if="product" class="product-detail">

    <!-- Product Image Section -->
    <v-container class="image-section pa-0">
      <div class="image-overlay d-flex align-center justify-space-between">
        <v-btn icon size="small" rounded="0" variant="text" class="overlay-btn" @click="goBack">
          <v-icon size="18">mdi-arrow-left</v-icon>
        </v-btn>
        <div class="d-flex">
          <v-btn icon size="small" rounded="0" variant="text" class="overlay-btn mr-2">
            <v-icon>mdi-share-variant</v-icon>
          </v-btn>
          <v-btn icon size="small" rounded="0" variant="text" class="overlay-btn" @click="toggleFavorite">
            <v-icon :color="isFavorited ? 'error' : undefined">
              {{ isFavorited ? 'mdi-heart' : 'mdi-heart-outline' }}</v-icon>
          </v-btn>
        </div>
      </div>
      <v-img :src="product.image" :height="isMobile ? 250 : 400" contain class="bg-white" />

      <!-- Image Dots Indicator -->
      <div class="text-center py-2">
        <span v-for="i in 5" :key="i" class="dot" :class="{ active: i === 1 }"></span>
      </div>
    </v-container>

    <!-- Product Information -->
    <v-container class="product-info bg-white rounded-t-xl mt-n4" style="position: relative; z-index: 1;">
      <div class="pt-6">
        <h1 class="product-title mb-2">{{ product.title }}</h1>
        <div v-if="isMobile" class="price-options-mobile d-flex align-center justify-space-between mb-4">
          <h2 class="product-price  mb-0">{{ formatPrice(product.price) }}</h2>
          <v-chip-group v-model="selectedOption" mandatory class="mobile-chip-group">
            <v-chip value="1TB" variant="outlined" size="small">1 TB</v-chip>
            <v-chip value="256MB" variant="outlined" size="small">256 MB</v-chip>
            <v-chip value="512MB" variant="outlined" size="small">512 MB</v-chip>
          </v-chip-group>
        </div>
        <!-- <h2 v-else class="product-price font-weight-bold mb-4">{{ formatPrice(product.price) }}</h2> -->

        <!-- Storage/Size Options -->
        <!-- <div class="mb-4" v-if="!isMobile">
          <h4 class="text-subtitle-1 mb-2">Available Options:</h4>
          <v-chip-group v-model="selectedOption" mandatory>
            <v-chip value="1TB" variant="outlined">1 TB</v-chip>
            <v-chip value="256MB" variant="outlined">256 MB</v-chip>
            <v-chip value="512MB" variant="outlined">512 MB</v-chip>
          </v-chip-group>
        </div> -->

        <!-- Seller Information -->
        <v-card class="seller-card mb-4 d-flex align-center">
          <v-card-text class="pa-4">
            <div class="d-flex align-center">
              <v-avatar size="26" class="mr-3">
                <v-img src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg" />
              </v-avatar>
              <div class="flex-grow-1">
                <div class="d-flex justify-space-between">
                  <div class="d-flex align-center">
                    <p class="font-weight-medium mb-1">Khalid S.</p>
                    <v-icon color="green" size="24">mdi-check-circle</v-icon>
                  </div>
                  <div class="d-flex align-center">
                    <v-icon color="orange" size="16" class="mr-1">mdi-star</v-icon>
                    <span class="text-body-2">{{ product.rating.rate }} | <span class="text-grey">{{
                        product.rating.count }} Reviews</span></span>
                  </div>
                </div>
              </div>
            </div>
          </v-card-text>
        </v-card>


        <!-- Product Description -->
        <div class="description-section mb-6">
          <p class="text-body-1 description-text">
            {{ product.description }}
          </p>
        </div>

        <v-divider class="mx-2" />
        <v-spacer style="height:16px;"></v-spacer>

        <!-- Delivery Options -->
        <div class="delivery-section mb-6 ">
          <h3 class="text-h6 mb-3 font-weight-bold">Delivery Options</h3>
          <v-card class="delivery-card">
            <v-card-text class="pa-3">
              <div class="d-flex align-center">
                <div class="flex-grow-1">
                  <p class="font-weight-medium mb-1">Villa No. 278 Khalifa City Park</p>
                  <p class="font-weight-medium mb-1">Abu Dhabi</p>
                  <p class="font-weight-medium mb-1">Unlted Arab Emirates</p>
                </div>
                <v-btn icon rounded="lg" width="40" height="40">
                  <v-icon size="20">mdi-pencil</v-icon>
                </v-btn>
              </div>
            </v-card-text>
          </v-card>
        </div>

        <v-container class="pa-4" fluid>
          <!-- Delivery Options -->
          <v-card class="pa-3 mb-4" elevation="0">
            <div class="d-flex">
              <!-- Standard Delivery -->
              <v-card class="pa-3 mr-2 flex-grow-1 text-center d-flex flex-column justify-center align-center"
                rounded="xl" width="150" height="140">
                <h4 class="font-weight-large mb-1">Standard Delivery</h4>
                <div class="text-body-2 mb-2">5 - 7 days</div>
                <v-chip class="custom-chip" size="small">Free</v-chip>
              </v-card>

              <!-- Express Delivery -->
              <v-card class="pa-3 flex-grow-1 text-center d-flex flex-column justify-center align-center" color="yellow"
                rounded="xl" elevation="0" width="150" height="140">
                <h4 class="font-weight-medium" style="color: black;">Express Delivery</h4>
                <div class="text-body-2 mb-2" style="color: black;">1 - 2 days</div>
                <v-chip class="custom-chip-dark" size="small">+15 IQD</v-chip>
              </v-card>
            </div>
          </v-card>

          <v-divider class="mx-2" />
          <v-spacer style="height:16px;"></v-spacer>

          <!-- Product Details -->
          <h3 class="text-h6 font-weight-bold mb-3">Product Details</h3>

          <v-list density="compact">

            <v-list-item>
              <v-list-item-title class="font-weight-medium">Brand</v-list-item-title>
              <v-chip class="ml-auto d-flex grey-bg" variant="outlined flat" size="medium">American Eagles
                Outfitters</v-chip>
            </v-list-item>

            <v-list-item>
              <v-list-item-title class="font-weight-medium">Model Name</v-list-item-title>
              <v-chip class="ml-auto d-flex grey-bg" variant="outlined flat" size="medium">
                iPhone 14 Pro Max 512GB Silver 5G With FaceTime <br> - Middle East Version
              </v-chip>
            </v-list-item>

            <v-list-item>
              <v-list-item-title class="font-weight-medium">Wireless Carrier</v-list-item-title>
              <v-chip class="ml-auto d-flex grey-bg" variant="outlined flat" size="medium">Unlocked for All
                Carriers</v-chip>
            </v-list-item>

            <v-list-item>
              <v-list-item-title class="font-weight-medium">Memory</v-list-item-title>
              <v-chip class="ml-auto d-flex grey-bg" variant="outlined flat" size="medium">No Expandable Memory</v-chip>
            </v-list-item>

            <v-list-item>
              <v-list-item-title class="font-weight-medium">Colors</v-list-item-title>
              <div class="ml-auto d-flex grey-bg" variant="outlined flat" size="medium">
                <v-avatar size="20" class="mr-2 " color="cyan"></v-avatar>
                <v-avatar size="20" class="mr-2" color="red lighten-3"></v-avatar>
                <v-avatar size="20" class="mr-2" color="grey lighten-1"></v-avatar>
                <v-avatar size="20" class="mr-2" color="yellow"></v-avatar>
              </div>
            </v-list-item>

            <v-list-item>
              <v-list-item-title class="font-weight-small">Camera Type</v-list-item-title>
              <v-chip class="ml-auto d-flex grey-bg" variant="outlined flat" size="medium">
                Primary Camera + Secondary Camera
              </v-chip>
            </v-list-item>

          </v-list>
        </v-container>

        <v-divider class="mx-2" />


        <v-container class="pa-4" fluid>
          <!-- Reviews Header -->
          <div class="d-flex justify-space-between align-center mb-4">
            <div>
              <h3 class="text-subtitle-1 font-weight-bold mb-1">Seller's Reviews</h3>
              <div class="text-body-2 text-grey">
                3,092 Reviews
              </div>
            </div>
            <div class="d-flex flex-column align-center">
              <span class=" mb-1">5.0</span>
              <v-rating :model-value="5" color="black" bg-color="grey-lighten-1" density="compact" readonly size="18"
                class="ml-2" />
            </div>
          </div>

          <!-- Review Card -->
          <div class="d-flex scroll-hide pb-2" style="gap: 16px;">
            <!-- First Review Card -->
            <v-card class="pa-3 custom-card flex-shrink-0" variant="outlined" rounded="xl" height="220" width="320">
              <div class="d-flex align-center mb-2 inner-no-shadow">
                <!-- Avatar -->
                <v-avatar size="32" class="mr-2">
                  <v-img src="https://randomuser.me/api/portraits/men/32.jpg" />
                </v-avatar>

                <!-- Reviewer Info -->
                <div class="flex-grow-1">
                  <div class="d-flex justify-space-between align-center">
                    <p class="font-weight-medium mb-0">Ahmed Khalid</p>
                    <div class="d-flex align-center">
                      <v-icon color="orange" size="16" class="mr-1">mdi-star</v-icon>
                      <span class="font-weight-bold text-body-2">4.5</span>
                    </div>
                  </div>
                  <span class="text-caption text-grey">1 month ago</span>
                </div>
              </div>

              <!-- Review Text -->
              <p class="text-body-2 mb-0">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et
                dolore magna aliqua.
              </p>
            </v-card>

            <!-- Second Review Card -->
            <v-card class="pa-3 custom-card flex-shrink-0" variant="outlined" rounded="xl" height="220" width="320">
              <div class="d-flex align-center mb-2 inner-no-shadow">
                <v-avatar size="32" class="mr-2">
                  <v-img src="https://randomuser.me/api/portraits/women/44.jpg" />
                </v-avatar>

                <div class="flex-grow-1">
                  <div class="d-flex justify-space-between align-center">
                    <p class="font-weight-medium mb-0">Sara Ali</p>
                    <div class="d-flex align-center">
                      <v-icon color="orange" size="16" class="mr-1">mdi-star</v-icon>
                      <span class="font-weight-bold text-body-2">5.0</span>
                    </div>
                  </div>
                  <span class="text-caption text-grey">2 weeks ago</span>
                </div>
              </div>

              <p class="text-body-2 mb-0">
                Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium.
              </p>
            </v-card>
            <!-- Add more cards similarly -->
          </div>
        </v-container>

        <v-divider class="mx-2" />
        <v-spacer style="height:16px;"></v-spacer>

        <!-- Comments Section -->
        <div class="comments-section mb-6">
          <h3 class="text-h6 mb-3 font-weight-bold">Comments</h3>
          <!-- Add Comment Input -->
          <div class="add-comment mb-4">
            <v-text-field v-model="newComment" label="Add a Comment" variant="outlined"
              append-inner-icon="mdi-emoticon-outline" hide-details class="mb-2" />
            <div class="d-flex justify-end">
              <!-- Show button only if user typed something -->
              <v-btn v-if="newComment.trim()" color="primary" variant="outlined" size="small" @click="addComment">
                Post Comment
              </v-btn>
            </div>
          </div>

          <!-- Comments List -->
          <div class="comments-list">
            <div v-for="comment in comments" :key="comment.id" class="comment-item mb-4">
              <div class="d-flex">
                <v-avatar size="40" class="mr-3" tile rardius="xl">
                  <v-img :src="comment.avatar" />
                </v-avatar>
                <div class="flex-grow-1">
                  <div class="mb-1 d-flex flex-column">
                    <span class="font-weight-medium">{{ comment.name }}</span>
                    <span class="text-caption text-grey">{{ comment.timeAgo }}</span>
                  </div>
                  <p class="text-body-2 mb-2">{{ comment.text }}</p>
                  <div class="d-flex align-center">
                    <v-btn icon size="small" variant="text" @click="likeComment(comment.id)"
                      :color="comment.isLiked ? 'primary' : 'default'">
                      <v-icon size="16">mdi-thumb-up</v-icon>
                    </v-btn>
                    <span class="text-caption ml-1">{{ comment.likes }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Show More Comments Button -->
          <div class="text-center">
            <v-btn variant="outlined" color="grey" @click="showMoreComments" width="80%">
              Show More Comments
            </v-btn>
          </div>

          <!-- Action Buttons Section -->
          <div class="action-buttons-section mb-6">
            <v-row>
              <v-col cols="6">
                <v-btn size="large" block @click="addToCart" class="add-to-cart-btn" elevation="2">
                  Add to Cart
                </v-btn>
              </v-col>
              <v-col cols="6">
                <v-btn size="large" block @click="buyNow" class="buy-now-btn" elevation="2">
                  Buy Now
                </v-btn>
              </v-col>
            </v-row>
          </div>

        </div>

        <!-- Similar Products -->
        <div v-if="!isMobile" class="similar-products mb-6">
          <h3 class="text-h6 mb-3">Similar Products</h3>
          <v-row>
            <v-col v-for="similarProduct in similarProducts" :key="similarProduct.id" cols="6" md="3">
              <ProductCard :product="similarProduct" :is-desktop="!isMobile" />
            </v-col>
          </v-row>
        </div>
      </div>
    </v-container>
  </div>

  <!-- Loading State -->
  <div v-else class="loading-container text-center pa-8">
    <v-progress-circular indeterminate color="primary" size="64" />
    <p class="mt-4">Loading product details...</p>
  </div>

  <!-- Success Snackbar -->
  <v-snackbar v-model="showSuccessMessage" color="primary" timeout="3000">
    Product added to cart successfully!
    <template v-slot:actions>
      <v-btn variant="text" @click="showSuccessMessage = false">
        Close
      </v-btn>
    </template>
  </v-snackbar>


</template>

<script>
import { ref, onMounted, computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { useDisplay } from 'vuetify'
import { useProductStore } from '../stores/products'
import { useCartStore } from '../stores/cart'
import { useFavoritesStore } from '../stores/favorites.js'
import ProductCard from '../components/ProductCard.vue'

export default {
  name: 'ProductDetailPage',
  components: {
    ProductCard,
  },
  setup() {
    const route = useRoute()
    const router = useRouter()
    const { mobile } = useDisplay()
    const productStore = useProductStore()
    const cartStore = useCartStore()
    const favoritesStore = useFavoritesStore()

    const product = ref(null)
    const loading = ref(false)
    const selectedOption = ref('1TB')
    const showSuccessMessage = ref(false)
    const newComment = ref('')
    const comments = ref([
      {
        id: 1,
        name: 'Ahmed Khalid',
        timeAgo: '1 month ago',
        text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.',
        avatar: 'https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg',
        likes: 942,
        isLiked: false
      },
      {
        id: 2,
        name: 'Ahmed Khalid',
        timeAgo: '1 month ago',
        text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.',
        avatar: 'https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg',
        likes: 942,
        isLiked: false
      }
    ])

    // Responsive detection
    const isMobile = computed(() => mobile.value)

    // Check if product is favorited
    const isFavorited = computed(() => {
      if (!product.value) return false
      return favoritesStore.isFavorite(product.value.id)
    })

    // Get similar products (same category, different products)
    const similarProducts = computed(() => {
      if (!product.value) return []
      return productStore.products
        .filter(p => p.category === product.value.category && p.id !== product.value.id)
        .slice(0, 4)
    })

    /**
     * Go back to previous page
     */
    const goBack = () => {
      router.back()
    }

    /**
     * Toggle favorite status
     */
    const toggleFavorite = () => {
      if (product.value) {
        favoritesStore.toggleFavorite(product.value)
      }
    }

    /**
     * Add product to cart
     */
    const addToCart = () => {
      if (product.value) {
        cartStore.addToCart(product.value)
        showSuccessMessage.value = true

        // Open cart drawer on desktop
        if (!isMobile.value) {
          cartStore.openDrawer()
        }
      }
    }

    /**
     * Handle buy now action
     */
    const buyNow = () => {
      if (product.value) {
        cartStore.addToCart(product.value)
        router.push('/cart')
      }
    }

    /**
     * Contact seller action
     */
    const contactSeller = () => {
      alert('Contact seller feature - This would open a messaging interface')
    }

    /**
     * Add a new comment
     */
    const addComment = () => {
      if (newComment.value.trim()) {
        const comment = {
          id: Date.now(),
          name: 'You',
          timeAgo: 'Just now',
          text: newComment.value,
          avatar: 'https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg',
          likes: 0,
          isLiked: false
        }
        comments.value.unshift(comment)
        newComment.value = ''
      }
    }

    /**
     * Like/unlike a comment
     */
    const likeComment = (commentId) => {
      const comment = comments.value.find(c => c.id === commentId)
      if (comment) {
        comment.isLiked = !comment.isLiked
        comment.likes += comment.isLiked ? 1 : -1
      }
    }

    /**
     * Show more comments
     */
    const showMoreComments = () => {
      // This would typically load more comments from an API
      alert('Loading more comments...')
    }

    /**
     * Add product to cart page
     */
    const addToCartPage = () => {
      if (product.value) {
        cartStore.addToCart(product.value)
        showSuccessMessage.value = true

        // Navigate to cart page
        router.push('/cart')
      }
    }

    /**
     * Format price in IQD currency
     * @param {number} price - Price in USD
     * @returns {string} Formatted price
     */
    const formatPrice = (price) => {
      return `${Math.round(price * 25)} IQD`
    }

    // Load product data on component mount
    onMounted(async () => {
      const productId = parseInt(route.params.id)
      loading.value = true

      try {
        product.value = await productStore.fetchProductById(productId)

        // Load all products if not already loaded (for similar products)
        if (productStore.products.length === 0) {
          await productStore.fetchProducts()
        }
      } catch (error) {
        console.error('Error loading product:', error)
        // Handle error - maybe redirect to 404 page
      } finally {
        loading.value = false
      }
    })

    return {
      // Reactive data
      product,
      loading,
      selectedOption,
      showSuccessMessage,
      newComment,
      comments,

      // Computed
      isMobile,
      isFavorited,
      similarProducts,

      // Methods
      goBack,
      toggleFavorite,
      addToCart,
      buyNow,
      contactSeller,
      addComment,
      likeComment,
      showMoreComments,
      addToCartPage,
      formatPrice,
    }
  },
}
</script>

<style scoped>
.product-detail {
  /* min-height: 100vh; */
  background-color: #f5f5f5;
}

.detail-header {
  position: sticky;
  top: 0;
  z-index: 10;
  border-bottom: 1px solid #e0e0e0;
}

.image-section {
  background: white;
}

.image-overlay {
  position: absolute;
  top: 8px;
  left: 0;
  right: 0;
  z-index: 2;
  padding: 8px 12px;
}

.overlay-btn:deep(.v-btn__overlay),
.overlay-btn:deep(.v-ripple__container) {
  display: none;
  border: #1a1a1a;
  border-radius: 10px;
}

.overlay-btn {
  background: rgba(255, 255, 255, 0.7) !important;
  backdrop-filter: blur(6px);
}

.dot {
  height: 8px;
  width: 8px;
  margin: 0 2px;
  background-color: #bbb;
  border-radius: 50%;
  display: inline-block;
  transition: background-color 0.3s;
}

.dot.active {
  background-color: #aee2dd;
}

.product-info {
  border-radius: 24px 24px 0 0;
  box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.1);
}

.product-title {
  font-size: 1.5rem;
  line-height: 1.3;
  color: #1a1a1a;
}

.product-price {
  font-size: 2rem;
  color: #000000;
}

.seller-card {
  border-radius: 12px;
  /* width: 0px;
  height: 49px; */
  border: none;
  background-color: #dcdcdc;
  box-shadow: 0;
}


.description-text {
  line-height: 1.6;
  color: #555;
}

.delivery-card {
  border-radius: 12px;
  border: none;
  background-color: #dcdcdc;
  box-shadow: 0;
}

.comments-section {
  background: white;
}

.comment-item {
  padding: 16px;
  border-radius: 12px;
  background: #ffffff;
  transition: all 0.2s ease;
}

.comment-item:hover {
  background: #ffffff;
}

.add-comment {
  background: #ffffff;
  padding: 16px;
  border-radius: 12px;
}

.action-buttons-section {
  margin-top: 24px;
}

.add-to-cart-btn {
  background-color: #4A4A4A !important;
  color: white !important;
  font-weight: 600;
  text-transform: none;
  border-radius: 12px;
  transition: all 0.3s ease;
  height: 48px;
}

.add-to-cart-btn:hover {
  background-color: #3A3A3A !important;
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.buy-now-btn {
  background-color: #00A896 !important;
  color: white !important;
  font-weight: 600;
  text-transform: none;
  border-radius: 12px;
  transition: all 0.3s ease;
  height: 48px;
}

.buy-now-btn:hover {
  background-color: #009688 !important;
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}





.loading-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

/* Inline price + options on mobile */
.price-options-mobile {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}

/* Make chips compact and evenly spaced */
.mobile-chip-group {
  display: flex;
  align-items: center;
}

.mobile-chip-group:deep(.v-chip) {
  margin-left: 8px;
  border-radius: 12px;
  border:none;

}
.custom-card {
  box-shadow: none !important;
  /* remove any elevation shadow */
  border: 1px solid #ddd;
  /* thin border */
}

/* Inner element without shadow */
.inner-no-shadow {
  box-shadow: none !important;
  /* remove any shadow */
  /* optional: subtle inner border */
  border-radius: 8px;
  /* optional: keep rounded corners */
  padding: 8px;
}
/* Mobile specific adjustments */
@media (max-width: 600px) {
  .product-title {
    font-size: 1.25rem;
  }

  .product-price {
    font-size: 1.75rem;
  }

  .fixed-bottom-actions {
    padding: 12px 16px;
  }
}

/* Desktop specific adjustments */
@media (min-width: 960px) {
  .product-info {
    max-width: 1200px;
    margin: 0 auto;
    border-radius: 16px;
  }

  .fixed-bottom-actions {
    max-width: 1200px;
    margin: 0 auto;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 16px 16px 0 0;
  }
}
.beautiful-btn {
  background: none !important;
  border-width: 0.5px !important;
  font-weight: 300;
  font-size: 13px;
  letter-spacing: 1px;
  border-radius: 12px;
  box-shadow: none !important;
  width: 400px;
  height: 50px;
}

.grey-bg {
    background-color: #e0e0e0;
      border: none;
      box-shadow: none;
      font-weight: bold;
      border-radius: 6px;
      font-size: 14px;
      padding: 6px 12px;
      color: black;
}
.custom-chip {
  background-color: #424242;
  color: rgb(255, 255, 255);
  border: none;
  box-shadow: none;
  font-weight: bold;
  border-radius: 15px;
  /* color: grey; */
}

.custom-chip-dark {
  background-color: #424242;
  color: white;
  border: none;
  box-shadow: none;
  font-weight: bold;
  border-radius: 15px;
}

.scroll-hide {
  overflow-x: auto;
  -ms-overflow-style: none;
  /* IE and Edge */
  scrollbar-width: none;
  /* Firefox */
}

.scroll-hide::-webkit-scrollbar {
  display: none;
  /* Chrome, Safari, Opera */
}
</style>