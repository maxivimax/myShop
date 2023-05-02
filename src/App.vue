<script setup lang="ts">
import Cart from './components/Cart.vue.js'
</script>

<script lang="ts">
export class Product {
  id: number;
  name: string;
  price: number;
  quantity: number;
  imageSrc: string;

  constructor(id: number, name: string, price: number, quantity: number, imageSrc: string) {
    this.id = id;
    this.name = name;
    this.price = price;
    this.quantity = quantity;
    this.imageSrc = imageSrc;
  }
}

// Функция для добавления товара в корзину
function addToCart(product: Product, cart: Product[]): Product[] {
  const existingProduct = cart.find(p => p.id === product.id);

  if (existingProduct) {
    existingProduct.quantity += 1;
  } else {
    cart.push({ ...product, quantity: 1 });
  }

  return cart;
}

// Функция для удаления товара из корзины
function removeFromCart(product: Product, cart: Product[]): Product[] {
  const existingProduct = cart.find(p => p.id === product.id);

  if (existingProduct) {
    existingProduct.quantity -= 1;

    if (existingProduct.quantity === 0) {
      return cart.filter(p => p.id !== product.id);
    }
  }

  return cart;
}

const products: Product[] = [
  new Product(1, 'Книга "Мастер и Маргарита"', 500, 1, "https://img4.labirint.ru/rc/7c2ecc71bdfcaea92d565a17f0773c34/363x561q80/books77/761472/cover.jpg?1606829583"),
  new Product(2, 'CD "The Dark Side of the Moon"', 1000, 1, "https://static.insales-cdn.com/r/wrmcKejGP2w/rs:fit:600:0:1/plain/images/products/1/4138/370077738/pink-floyd-the-dark-side-of-the-moon-mini-lp-cd__3_.jpg"),
  new Product(3, 'Молоко 1л', 50, 1, "https://roscontrol.com/wp-content/uploads/2021/09/e7b17f6a38a36f3af2d7.jpg")
];

let cart: Product[] = [];
</script>

<template>
  <Cart v-bind:products="cart" v-bind:add="addToCart" v-bind:remove="removeFromCart" />

  <div>
    <div class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:max-w-7xl lg:px-8">
      <h2 class="text-2xl font-bold tracking-tight text-white">Товары</h2>

      <div class="mt-6 grid grid-cols-1 gap-x-6 gap-y-10 sm:grid-cols-2 lg:grid-cols-4 xl:gap-x-8">
        <div v-for="product in products" :key="product.id" @click="addToCart(product, cart)" class="group relative">
          <div
            class="min-h-80 aspect-h-1 aspect-w-1 w-full overflow-hidden rounded-md bg-gray-200 lg:aspect-none group-hover:opacity-75 lg:h-80">
            <img :src="product.imageSrc" class="h-full w-full object-cover object-center lg:h-full lg:w-full" />
          </div>
          <div class="mt-4 flex justify-between">
            <div>
              <h3 class="text-sm text-white">
                <a>
                  <span aria-hidden="true" class="absolute inset-0" />
                  {{ product.name }}
                </a>
              </h3>
            </div>
            <p class="text-sm font-medium text-white">{{ product.price }} ₽</p>
          </div>
          <p class="text-sm font-bold text-white">Нажмите для добавления в корзину</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
