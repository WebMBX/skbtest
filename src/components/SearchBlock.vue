<template>
  <div class="container">
    <div class="search-cont">
      <form action="" v-on:submit.prevent="sendForm" ref="searchForm">
        <div class="main-search">
          <select
            name="city"
            v-model="city"
            class="main-search__select left-rounded"
            v-show="isShowMainSearch"
          >
            <option v-for="(option, index) in cityOptions" :value="option.value" :key="index">
              {{ option.text }}
            </option>
          </select>
          <select
            name="operation"
            v-model="operation"
            class="main-search__select mlr-1"
            v-show="isShowMainSearch"
          >
            <option v-for="(option, index) in operationOptions" :value="option.value" :key="index">
              {{ option.text }}
            </option>
          </select>
          <select
            name="object"
            v-model="object"
            class="main-search__select"
            v-show="isShowMainSearch"
          >
            <option v-for="(option, index) in objectOptions" :value="option.value" :key="index">
              {{ option.text }}
            </option>
          </select>
          <input
            type="text"
            v-model="searchText"
            name="text"
            class="main-search__search-input"
            placeholder="Введите название объекта (бизнес центра,торгового центра, новостройки)"
            v-show="!isShowMainSearch"
          />
          <button class="main-search__button right-rounded" type="submit">Найти</button>
        </div>
        <div class="options-search" v-show="isShowPriceSearch || isShowMeasureSearch">
          <div class="options-search__left-side">
            <button
              type="button"
              class="options-search__measure-button rounded"
              @click="showPriceSearch"
              v-show="isShowMeasureSearch && !isShowPriceSearch"
            >
              Добавить цену
            </button>

            <div class="price-search" v-show="isShowPriceSearch">
              <input
                v-model="priceFrom"
                type="text"
                name="priceFrom"
                placeholder="От"
                class="price-search__input left-rounded"
              />
              <input
                v-model="priceTo"
                type="text"
                name="priceTo"
                placeholder="До"
                class="price-search__input mlr-1"
              />
              <select
                v-model="priceType"
                name="priceType"
                class="price-search__select right-rounded"
              >
                <option v-for="(option, index) in priceOptions" :value="option.value" :key="index">
                  {{ option.text }}
                </option>
              </select>
            </div>
          </div>

          <div class="options-search__right-side">
            <button
              type="button"
              class="options-search__price-button rounded"
              @click="showMeasureSearch"
              v-show="isShowPriceSearch && !isShowMeasureSearch"
            >
              Добавить метраж
            </button>

            <div class="measure-search" v-show="isShowMeasureSearch">
              <input
                v-model="sizeFrom"
                type="text"
                placeholder="От"
                name="measureFrom"
                class="measure-search__input left-rounded"
              />
              <input
                v-model="sizeTo"
                type="text"
                placeholder="До"
                name="measureTo"
                class="measure-search__input mlr-1"
              />
              <span class="measure-search__postfix"
                ><span>м<sup>2</sup></span></span
              >
            </div>
          </div>
        </div>
      </form>
    </div>
    <div class="footer-options">
      <div class="footer-options__left">
        <div class="switcher-cont">
          <span
            :class="[
              'switcher-cont__item',
              isShowMainSearch ? 'switcher-cont__item--active' : null
            ]"
            @click="showMainSearch(true)"
            >Основной поиск</span
          >
          <label class="switch">
            <input type="checkbox" id="switch" v-model="isShowMainSearch" />
            <span class="switcher-cont__slider slider round"></span>
          </label>
          <span
            :class="[
              'switcher-cont__item',
              !isShowMainSearch ? 'switcher-cont__item--active' : null
            ]"
            @click="showMainSearch(false)"
            >Искать по названию</span
          >
        </div>
      </div>

      <div class="footer-options__right">
        <button
          type="button"
          class="footer-options__right__button"
          :class="[
            'footer-options__right__button',
            isShowPriceSearch ? 'footer-options__right__button--active' : null
          ]"
          @click="showPriceSearch(!isShowPriceSearch)"
        >
          Цена
        </button>
        <button
          type="button"
          :class="[
            'footer-options__right__button',
            isShowMeasureSearch ? 'footer-options__right__button--active' : null
          ]"
          @click="showMeasureSearch(!isShowMeasureSearch)"
        >
          Метраж
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: 'moscow',
      cityOptions: [
        { text: 'Москва', value: 'moscow' },
        { text: 'Санкт-Петербург', value: 'spb' },
        { text: 'Казинь', value: 'kazan' },
      ],
      operation: 'buy',
      operationOptions: [
        { text: 'Купить', value: 'buy' },
        { text: 'Продать', value: 'sell' },
        { text: 'Арендовать', value: 'rent' },
      ],
      object: 'office',
      objectOptions: [
        { text: 'Квартиру', value: 'apartment' },
        { text: 'Дом', value: 'house' },
        { text: 'Офис', value: 'office' },
      ],
      priceType: '1',
      priceOptions: [
        { text: '₽/Месяц', value: '1' },
        { text: '₽/Год', value: '2' },
      ],
      searchText: '',
      priceFrom: '',
      priceTo: '',
      sizeFrom: '',
      sizeTo: '',

      isShowPriceSearch: false,
      isShowMeasureSearch: false,
      isShowMainSearch: true,
      data: {},
    };
  },
  methods: {
    showMainSearch(status) {
      this.isShowMainSearch = !!status;
    },
    showMeasureSearch(status = true) {
      this.isShowMeasureSearch = status;
    },
    showPriceSearch(status = true) {
      this.isShowPriceSearch = status;
    },
    sendForm() {
      // Вариант 1
      const formData = Array.from(new FormData(this.$refs.searchForm), (e) => e.map(encodeURIComponent).join('=')).join('&');

      console.log('FormData = ', formData);

      // Вариант 2
      let data = this.isShowMainSearch
        ? {
          city: this.city,
          action: this.operation,
          object: this.object,
        }
        : { searchText: this.searchText };
      if (this.isShowPriceSearch) {
        data = {
          ...data,
          priceFrom: this.priceFrom,
          priceTo: this.priceTo,
          priceType: this.priceType,
        };
      }
      if (this.isShowMeasureSearch) {
        data = {
          ...data,
          sizeFrom: this.sizeFrom,
          sizeTo: this.sizeTo,
        };
      }
      console.log('VueData = ', data);
    },
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/switch.scss";
.container {
  margin: 0 auto;
  max-width: 1280px;
  background: linear-gradient(90deg, #83b3e5, #a8d5e9);
}

button {
  &:focus {
    outline: none;
  }
}

select {
  -webkit-appearance: none;
  -moz-appearance: none;
  border: none;
  background: #fff;
  padding: 10px 20px;
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-image: url("data:image/svg+xml;charset=utf-8, \
    <svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 60 40'> \
      <polygon points='0,0 60,0 30,40' style='fill:black;'/> \
    </svg>");
  background-size: 6px;
  height: 44px;
  &:focus {
    outline: none;
  }
}

input[type="text"] {
  -webkit-appearance: none;
  -moz-appearance: none;
  border: none;
  background: #fff;
  padding: 0 20px;
  height: 44px;

  &:focus {
    outline: none;
  }
}

button:hover {
  cursor: pointer;
}

.search-cont {
  min-width: 690px;
  margin: 0 auto;
  padding: 30px;
}

.left-rounded {
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
}

.right-rounded {
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
}
.mlr-1 {
  margin: 0 1px 0 1px;
}

.rounded {
  border-radius: 3px !important;
}

.main-search {
  display: flex;
  margin: 0 auto;
  justify-content: center;
  width: 100%;
  &__select {
    flex: 1;
    &nth:nth-child(1) {
      border-top-left-radius: 6px;
      border-bottom-left-radius: 6px;
    }
  }
  &__button {
    background: #0ca975;
    color: #fff;
    padding: 10px 22px;
    border: none;
    height: 44px;
    &:hover {
      opacity: 0.9;
    }
  }
  &__search-input {
    flex: 1;
  }
}

.options-search {
  margin-top: 24px;
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-between;
  align-items: center;

  &__left-side {
    display: flex;
    flex: 3;
    margin-right: 12px;
  }

  &__right-side {
    display: flex;
    flex: 2;
  }
  .options-search__price-button {
    background: transparent;
    color: #111;
    padding: 10px 18px;
    border: 1px solid #111;
    height: 44px;
    flex: 1;
  }
  .options-search__measure-button {
    background: transparent;
    color: #111;
    padding: 10px 18px;
    border: 1px solid #111;
    height: 44px;
    flex: 3;
  }
}

.price-search {
  display: flex;
  justify-content: center;
  width: 100%;
  flex: 3;

  &__input {
    flex: 1;
    background: rgb(255 255 255 / 90%);
  }

  &__select {
    flex: 1;
    background-color: rgb(255 255 255 / 90%);
  }
}

.measure-search {
  display: flex;
  justify-content: center;
  width: 100%;
  flex: 1;

  &__input {
    flex: 1;
    background: rgb(255 255 255 / 90%);
  }
  &__postfix {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    padding: 0 14px;
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;
    background: rgb(255 255 255 / 70%);
    sup {
      font-size: 0.6em;
    }
  }
}

.footer-options {
  background: #e6f2fe;
  padding: 15px;
  display: flex;
  justify-content: space-between;
}

.switcher-cont {
  &__item {
    font-size: 0.8rem;
    color: #111;
    border-bottom: 1px dashed #555;
    cursor: pointer;
  }
  &__item--active {
    color: #828282;
    border: none;
    cursor: default;
  }
  &__slider {
    transform: rotate(180deg);
  }
}

.footer-options__right {
  &__button {
    margin: 0 4px;
    background: none;
    color: #111;
    padding: 4px 14px;
    border: none;
    &::before {
      content: "+";
      margin-right: 2px;
      border: 2px solid #555;
      color: #555;
      border-radius: 100%;
      font-size: 14px;
      font-weight: bold;

      display: inline-flex;
      justify-content: center;
      align-items: center;
      width: 14px;
      height: 14px;
      padding: 0;
    }
  }
  &__button--active {
    color: #555;
    &::before {
      transform: rotateZ(45deg);
    }
  }
}
</style>
