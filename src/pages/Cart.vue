<template>
  <v-container class="cart-page pa-4">
    <div class="d-flex align-center justify-center mb-6 position-relative">
      <!-- Back button fixed left -->
      <v-btn icon @click="goBack" class="position-absolute left-0">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>

      <!-- Centered title -->
      <h1 class="text-h6 m-0">My Cart</h1>
    </div>

    <div v-if="cartStore.isEmpty" class="empty-cart text-center py-12">
      <v-icon size="120" color="grey-lighten-2">mdi-cart-outline</v-icon>
      <h3 class="text-h5 mt-6 mb-3">Your cart is empty</h3>
      <p class="text-body-1 mb-6 text-grey-darken-1">Add some products to get started shopping</p>
      <v-btn color="primary" size="large" @click="startShopping" prepend-icon="mdi-shopping">
        Start Shopping
      </v-btn>
    </div>

    <div v-else class="cart-content">
      <div class="cart-items mb-4">
        <v-card v-for="item in cartStore.items" :key="item.product.id" class="cart-item-card mb-4" variant="outlined">
          <v-card-text class="pa-4">
            <v-row align="center">
              <v-col cols="3" sm="2">
                <v-img :src="item.product.image" height="80" class="rounded cart-item-image of-contain"
                  @click="goToProduct(item.product.id)" />
              </v-col>

              <v-col cols="6" sm="7">
                <h3 class="cart-item-title mb-2" @click="goToProduct(item.product.id)">
                  {{ truncateTitle(item.product.title) }}
                </h3>
                <div class="d-flex align-center mb-2">
                  <v-avatar size="20" class="mr-2">
                    <v-img src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg" />
                  </v-avatar>
                  <span class="text-body-2">Khalid S.</span>
                  <v-icon color="green" size="16" class="ml-1">mdi-check-circle</v-icon>
                </div>
                <p class="cart-item-price font-weight-bold mb-0">
                  {{ formatPrice(item.product.price) }}
                </p>
              </v-col>

              <v-col cols="3" sm="3" class="text-center">
                <!-- Delete Button on Top -->
                <div class="d-flex align-right justify-right">
                  <v-btn icon size="small" color="error" rounded="0" variant="text" @click="removeItem(item.product.id)"
                    class="mb-3">
                    <v-icon size="18">mdi-delete</v-icon>
                  </v-btn>
                </div>
                <!-- Quantity Controls Below -->
                <div class="quantity-controls d-flex align-center justify-center">
                  <v-btn icon size="s" rounded="0" variant="outlined"
                    @click="decreaseQuantity(item.product.id, item.quantity)" :disabled="item.quantity <= 1">
                    <v-icon size="16">mdi-minus</v-icon>
                  </v-btn>
                  <span class="quantity-display mx-3 font-weight-bold">{{ item.quantity }}</span>
                  <v-btn icon size="xs" rounded="0" variant="outlined"
                    @click="increaseQuantity(item.product.id, item.quantity)">
                    <v-icon size="16">mdi-plus</v-icon>
                  </v-btn>
                </div>
              </v-col>



            </v-row>
          </v-card-text>
        </v-card>
        <v-divider class="mx-3" />

      </div>

      <!-- One card, two bodies: desktop/tablet (dynamic) vs mobile (static like screenshot) -->
      <v-card class="order-summary-card flat mb-0" variant="outlined">
        <v-card-text class="pt-2 order-summary-desktop">
          <div class="summary-row d-flex justify-space-between mb-2">
            <span>Subtotal ({{ cartStore.totalItems }} items)</span>
            <span>{{ formatIQD(cartStore.subtotal) }}</span>
          </div>
          <div class="summary-row d-flex justify-space-between mb-2">
            <span>Discount (10%)</span>
            <span>-{{ formatIQD(cartStore.discount) }}</span>
          </div>
          <div class="summary-row d-flex justify-space-between mb-4">
            <span>Shipping Fee</span>
            <span>{{ formatIQD(cartStore.shippingFee) }}</span>
          </div>
          <v-divider class="mb-4" />
          <div class="total-row d-flex justify-space-between text-h6 font-weight-bold">
            <span>Total <span class="text-body-2 font-weight-normal text-grey-darken-1">(Tax Inclusive)</span></span>
            <span class="text-primary">{{ formatIQD(cartStore.total) }}</span>
          </div>
        </v-card-text>

        <v-card-text class="pt-2 order-summary-mobile">
          <div class="summary-row d-flex justify-space-between mb-2">
            <span>Subtotal</span>
            <span>1,000 IQD</span>
          </div>
          <div class="summary-row d-flex justify-space-between mb-2 ">
            <span>Discount</span>
            <span>23.0 IQD</span>
          </div>
          <div class="summary-row d-flex justify-space-between mb-4">
            <span>Shipping Fee</span>
            <span>41.0 IQD</span>
          </div>
          <v-divider class="mb-4" />
          <div class="total-row d-flex justify-space-between text-h6 font-weight-bold">
            <span>Total <span class="text-body-2 font-weight-normal text-grey-darken-1">Tax Inclusive</span></span>
            <span>1,057 IQD</span>
          </div>
        </v-card-text>
        <div class="cart-actions d-flex align-center justify-center">
          <v-row class="d-flex align-center justify-center">
            <v-col class="d-flex align-center justify-center">
              <v-btn color="primary" size="large"  class="d-flex align-center justify-center">
                Proceed to Checkout
              </v-btn>
            </v-col>
          </v-row>
        </div>
      </v-card>


    </div>

  </v-container>
</template>

<script>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useCartStore } from '../stores/cart'

export default {
  name: 'CartPage',
  setup() {
    const router = useRouter()
    const cartStore = useCartStore()

    const checkoutLoading = ref(false)
    const showClearDialog = ref(false)

    const goBack = () => router.back()
    const startShopping = () => router.push('/')
    const continueShopping = () => router.push('/')
    const goToProduct = (productId) => router.push(`/product/${productId}`)
    const removeItem = (productId) => cartStore.removeFromCart(productId)
    const increaseQuantity = (productId, currentQuantity) => {
      cartStore.updateQuantity(productId, currentQuantity + 1)
    }
    const decreaseQuantity = (productId, currentQuantity) => {
      if (currentQuantity > 1) cartStore.updateQuantity(productId, currentQuantity - 1)
    }
    const proceedToCheckout = () => {
      checkoutLoading.value = true
      setTimeout(() => {
        checkoutLoading.value = false
        alert('Checkout functionality would be implemented here. Redirecting to payment gateway...')
      }, 1200)
    }
    const clearCart = () => { showClearDialog.value = true }
    const confirmClearCart = () => { cartStore.clearCart(); showClearDialog.value = false }

    const truncateTitle = (t) => (t.length > 30 ? t.slice(0, 30) + '...' : t)
    const formatIQD = (n) =>
      `${Number(n).toLocaleString('en-US', { maximumFractionDigits: 0 })} IQD`
    const formatPrice = (usd) => formatIQD(Math.round(usd * 25))

    return {
      cartStore,
      checkoutLoading,
      showClearDialog,
      goBack,
      startShopping,
      continueShopping,
      goToProduct,
      removeItem,
      increaseQuantity,
      decreaseQuantity,
      proceedToCheckout,
      clearCart,
      confirmClearCart,
      truncateTitle,
      formatIQD,
      formatPrice,
    }
  },
}
</script>

<style scoped>
.cart-page {
  min-height: 100vh;
  background-color: #f5f5f5;
}

.empty-cart {
  min-height: 60vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.cart-item-card {
  box-shadow: none !important;
  border: none !important;
}

.cart-item-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.cart-item-image {
  cursor: pointer;
  transition: transform 0.2s ease;
}

.cart-item-image:hover {
  transform: scale(1.05);
}

.of-contain :deep(img) {
  object-fit: contain;
}

.cart-item-title {
  cursor: pointer;
  transition: color 0.2s ease;
  font-size: 1rem;
  line-height: 1.3;
}



.quantity-controls {
  gap: 8px;
}

.quantity-display {
  min-width: 30px;
  text-align: center;
  font-size: 1rem;
  backdrop-filter: blur(12px);
}

.order-summary-card {
  box-shadow: none !important;
  border: none !important;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1000;
}

.summary-row {
  font-size: 0.95rem;
  color:black
}

.total-row {
  font-size: 1.2rem;
}

.cart-actions {
  margin-bottom: 100px;
  /* width: 400px; */
  
}

/* show dynamic summary by default */
.order-summary-desktop {
  display: block;
}

.order-summary-mobile {
  display: none;
}

/* Mobile */
@media (max-width: 600px) {
  .cart-page {
    padding: 16px 8px;
  }

  .cart-item-title {
    font-size: 0.9rem;
  }

  .cart-item-price {
    font-size: 1rem;
  }

  .quantity-display {
    font-size: 0.9rem;

  }

  /* switch to the screenshot figures only on mobile */
  .order-summary-desktop {
    display: none;
  }

  .order-summary-mobile {
    display: block;
  }
}

/* Desktop */
@media (min-width: 960px) {
  .cart-actions {
    margin-bottom: 20px;
  }
}
</style>
