<template>
<div>
  <h1 class="content__title" style="text-align: center" v-if="productsLoading">
      Идет загрузка товаров...
  </h1>
  <h1 class="content__title" style="text-align: center" v-else-if="!productsData">
      Произошла ошибка, попробуйте еще раз, нажмите на кнопку.
      <button class="button button--second" @click="loadProducts">Попробовать</button>
  </h1>
  <main class="content container" v-if="productsData">
    <div class="content__top" >
        <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
            <router-link class="breadcrumbs__link" :to="{name: 'main'}">
            Каталог
            </router-link>
        </li>
        <li class="breadcrumbs__item">
            <router-link class="breadcrumbs__link" href="#" :to="{name: 'main'}">
            {{ categories.title }}
            </router-link>
        </li>
        <li class="breadcrumbs__item">
            <a class="breadcrumbs__link">
            {{ product.title }}
            </a>
        </li>
        </ul>
    </div>
    <section class="item">
        <div class="item__pics pics">
          <div class="pics__wrapper">
            <img width="570" height="570" :src="product.image.file.url" :alt="product.title">
          </div>
          <ul class="pics__list">
              <li class="pics__item">
                <a href="" class="pics__link pics__link--current">
                  <img width="98" height="98" :src="product.image.file.url"
                  alt="Название товара">
                </a>
              </li>
              <li class="pics__item">
                <a href="" class="pics__link">
                  <img width="98" height="98" :src="product.image.file.url"
                  alt="Название товара">
                </a>
              </li>
              <li class="pics__item">
                <a href="" class="pics__link">
                  <img width="98" height="98" :src="product.image.file.url"
                  alt="Название товара">
                </a>
              </li>
              <li class="pics__item">
                <a class="pics__link" href="#">
                  <img width="98" height="98" :src="product.image.file.url"
                  alt="Название товара">
                </a>
              </li>
          </ul>
        </div>
        <div class="item__info">
          <span class="item__code">Артикул: {{ product.id }}</span>
          <h2 class="item__title">
              {{ product.title }}
          </h2>
          <div class="item__form">
            <form class="form" action="#" method="POST" @submit.prevent="addToCart">
            <b class="item__price">
                {{ product.price | numFormat }} ₽
            </b>
            <fieldset class="form__block">
                <legend class="form__legend">Цвет:</legend>
                <ul class="colors">
                <li class="colors__item">
                    <label class="colors__label">
                      <input class="colors__radio sr-only"
                      type="radio" name="color-item" value="blue" checked="">
                      <span class="colors__value" style="background-color: #73B6EA;">
                      </span>
                    </label>
                </li>
                <li class="colors__item">
                    <label class="colors__label">
                      <input class="colors__radio sr-only"
                      type="radio" name="color-item" value="yellow">
                      <span class="colors__value" style="background-color: #FFBE15;">
                      </span>
                    </label>
                </li>
                <li class="colors__item">
                    <label class="colors__label">
                      <input class="colors__radio sr-only"
                      type="radio" name="color-item" value="gray">
                      <span class="colors__value" style="background-color: #939393;">
                      </span>
                    </label>
                </li>
                </ul>
            </fieldset>
            <fieldset class="form__block" v-if="product.capacitySize > 0">
                <legend class="form__legend">Объемб в ГБ:</legend>
                <ul class="sizes sizes--primery">
                <li class="sizes__item">
                    <label class="sizes__label">
                      <input class="sizes__radio sr-only"
                      type="radio" name="sizes-item" value="32">
                      <span class="sizes__value">
                          32gb
                      </span>
                    </label>
                </li>
                <li class="sizes__item">
                    <label class="sizes__label">
                      <input class="sizes__radio sr-only"
                      type="radio" name="sizes-item" value="64">
                      <span class="sizes__value">
                          64gb
                      </span>
                    </label>
                </li>
                <li class="sizes__item">
                    <label class="sizes__label">
                      <input class="sizes__radio sr-only"
                      type="radio" name="sizes-item" value="128" checked="">
                      <span class="sizes__value">
                          128gb
                      </span>
                    </label>
                </li>
                </ul>
            </fieldset>
            <div class="item__row">
                <div class="form__counter">
                <button type="button" aria-label="Убрать один товар" @click="productAmount--">
                    <svg width="12" height="12" fill="currentColor">
                    <use xlink:href="#icon-minus"></use>
                    </svg>
                </button>
                <input type="text" value="1" name="count" v-model.number="productAmount">
                <button type="button" aria-label="Добавить один товар" @click="productAmount++">
                    <svg width="12" height="12" fill="currentColor">
                    <use xlink:href="#icon-plus"></use>
                    </svg>
                </button>
                </div>
                <button class="button button--primery" type="submit" :disabled="productLoadingAddeded">
                В корзину
                </button>
                <div v-if="productAddeded">Товар добавлен в корзину</div>
                <div v-if="productLoadingAddeded">Идет добавление товара в корзину...</div>
            </div>
            </form>
          </div>
        </div>
        <div class="item__desc">
          <ul class="tabs">
              <li class="tabs__item">
              <a class="tabs__link tabs__link--current">
                  Описание
              </a>
              </li>
              <li class="tabs__item">
              <a class="tabs__link" href="#">
                  Характеристики
              </a>
              </li>
              <li class="tabs__item">
              <a class="tabs__link" href="#">
                  Гарантия
              </a>
              </li>
              <li class="tabs__item">
              <a class="tabs__link" href="#">
                  Оплата и доставка
              </a>
              </li>
          </ul>
          <div class="item__content">
              <p>
              Навигация GPS, ГЛОНАСС, BEIDOU Galileo и QZSS<br>
              Синхронизация со смартфоном<br>
              Связь по Bluetooth Smart, ANT+ и Wi-Fi<br>
              Поддержка сторонних приложений<br>
              </p>
              <a href="#">
              Все характеристики
              </a>
              <h3>Что это?</h3>
              <p>
                Wahoo ELEMNT BOLT GPS – это велокомпьютер, который позволяет
                оптимизировать свои велотренировки, сделав их максимально
                эффективными.Wahoo ELEMNT BOLT GPS синхронизируется с датчиками
                по ANT+, объединяя полученную с них информацию. Данные
                отображаются на дисплее, а также сохраняются на смартфоне.
                При этом на мобильное устройство можно установить как фирменное
                приложение, так и различные приложения сторонних разработчиков.
                Велокомпьютер точно отслеживает местоположение, принимая сигнал
                с целого комплекса спутников. Эта информация позволяет смотреть
                уже преодоленные маршруты и планировать новые велопрогулки.
                </p>
                <h3>Дизайн</h3>
                <p>
                Велокомпьютер Wahoo ELEMNT BOLT очень компактный. Размеры
                устройства составляют всего 74,6 x 47,3 x 22,1 мм. что не
                превышает габариты смартфона. Корпус гаджета выполнен из черного
                пластика. На обращенной к пользователю стороне расположен дисплей
                диагональю 56 мм. На дисплей выводятся координаты и скорость, а также
                полученная со смартфона и синхронизированных датчиков информация:
                интенсивность, скорость вращения педалей,
                пульс и т.д. (датчики не входят в комплект поставки).
                Корпус велокомпьютера имеет степень защиты от влаги IPX7.
                Это означает, что устройство не боится пыли, а также выдерживает
                кратковременное (до 30 минут) погружение в воду на глубину не более 1 метра.
              </p>
          </div>
        </div>
    </section>
  </main>
</div>

</template>

<script>
import axios from 'axios';
import { mapActions } from 'vuex';
import gotoPage from '../helpers/gotoPage';
import numFormat from '../helpers/numFormat';

export default {
  data() {
    return {
      productAmount: 1,
      productsData: null,
      productsLoading: false,
      productsLoadingError: false,

      productLoadingAddeded: false,
      productAddeded: false,
    };
  },
  filters: {
    numFormat,
  },
  computed: {
    product() {
      return this.productsData;
    },
    categories() {
      return this.productsData.category;
    },
  },
  methods: {
    ...mapActions(['addProductToCart']),
    gotoPage,
    addToCart() {
      this.productAddeded = false;
      this.productLoadingAddeded = true;
      this.addProductToCart({ productId: this.product.id, amount: this.productAmount })
        .then(() => {
          this.productLoadingAddeded = false;
          this.productAddeded = true;
        });

    },
    loadProducts() {
      this.productsLoading = true;
      this.productsLoadingError = false;
      axios
        .get('https://vue-study.skillbox.cc/api/products/' + this.$route.params.id)
        .then((response) => this.productsData = response.data)
        .catch(() => this.productsLoadingError = true)
        .then(() => this.productsLoading = false);
    },
  },
  watch: {
    '$route.params.id': {
      handler() {
        this.loadProducts();
      },
      immediate: true,
    },
  },
};
</script>
