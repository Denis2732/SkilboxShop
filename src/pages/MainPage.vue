<template>
  <div>
    <main class="content container">
      <div class="content__top content__top--catalog">
        <h1 class="content__title">
          Каталог
        </h1>
        <span class="content__info" v-if="countProducts > 1 && countProducts < 5">
          {{countProducts}} товара
        </span>
        <span class="content__info" v-if="countProducts > 5 || countProducts === 0">
          {{countProducts}} товаров
        </span>
      </div>

      <div class="content__catalog">
        <ProductFilter
        :price-from.sync="filterPriceFrom"
        :price-to.sync="filterPriceTo"
        :category-id.sync="filterCategoryId"
        :check.sync="filterCheck"
        :color.sync="filterColor"
        :page.sync="page"
        />
        <section class="catalog" style="align-items: center">
          <ProductList :products="products"/>
          <h1 class="content__title" style="text-align: center" v-if="countProducts === 0">
              Разраб сказал: "Таких товаров нет!"
          </h1>
          <BasePagination v-model="page" :count="countProducts" :per-page="productsPerPage"/>

      </section>

      </div>
    </main>

  </div>
</template>

<script>
import products from '@/data/products';
import ProductList from '@/components/ProductList.vue';
import BasePagination from '@/components/BasePagination.vue';
import ProductFilter from '@/components/ProductFilter.vue';

export default {
  components: { ProductList, BasePagination, ProductFilter },
  data() {
    return {
      filterPriceFrom: 0,
      filterPriceTo: 0,
      filterCategoryId: 0,
      filterColor: '',
      filterCheck: [],
      page: 1,
      productsPerPage: 6,
    };
  },
  computed: {
    filterProducts() {
      let filterProducts = products;
      if (this.filterPriceFrom > 0) {
        filterProducts = filterProducts.filter((product) => product.price > this.filterPriceFrom);
      }
      if (this.filterPriceTo > 0) {
        filterProducts = filterProducts.filter((product) => product.price < this.filterPriceTo);
      }
      if (this.filterCategoryId > 0) {
        filterProducts = filterProducts.filter(
          (product) => product.categoryId === this.filterCategoryId,
        );
      }
      if (this.filterCategoryId === 0) {
        filterProducts = filterProducts.filter(
          (product) => product.categoryIAll === this.filterCategoryId,
        );
      }
      if (this.filterColor) {
        filterProducts = filterProducts.filter(
          (product) => product.color === this.filterColor,
        );
      }
      if (this.filterCheck.length > 0) {
        let array = [];
        let array2 = [];
        this.filterCheck.forEach((element) => {
          array = filterProducts.filter(
            (product) => product.capacitySize === element,
          );
          array2 = array2.concat(array);
        });
        filterProducts = array2;
      }
      return filterProducts;
    },
    products() {
      const offset = (this.page - 1) * this.productsPerPage;
      return this.filterProducts.slice(offset, offset + this.productsPerPage);
    },
    countProducts() {
      return this.filterProducts.length;
    },
  },
};
</script>
