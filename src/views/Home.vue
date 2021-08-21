<template>
  <div>
   <Loader v-if="isLoading"></Loader>
    <div class="main" v-if="!isLoading">
      <div class="row">
        <div class="col-md-6 col-sm-6 col-lg-6" style="padding-bottom: 0px;">
          <button class="main__addbtn" @click="openAddModal">Добавить</button>
        </div>
        <div class="col-md-6 col-sm-6 col-lg-6" style="padding-bottom: 0px;">
          <v-select class="main__sort" :items="sorts" item-text="name" item-value="id" name="sort" id="sort" @change="onChange" solo placeholder="Сортировка"></v-select>
        </div>
      </div>
      <div class="row">
        <div class="col-md-4 col-sm-4" v-if="openDialog" style="margin-bottom: 20px;">
          <form action="" class="main__add">
            <div class="row">
              <div class="col-md-12 col-sm-12 col-lg-12" style="padding-bottom: 0px;">
                <label class="main__add__label" for="name">Наименование товара</label><i class="fas fa-circle fa-xs" style="color:#FF8484;margin-left:10px;"></i>
              </div>
              <div class="col-md-12 col-sm-12 col-lg-12" style="padding-bottom: 0px;">
                <v-text-field v-model="product.title" :rules="[rules.required]" id="name" placeholder="Введите наименование товара" required solo></v-text-field>
              </div>
              <div class="col-md-12 col-sm-12 col-lg-12" style="padding: 0px 12px;">
                <label class="main__add__label" for="desc">Oписание товара</label><i class="fas fa-circle fa-xs" style="color:#FF8484;margin-left:10px;"></i>
              </div>
              <div class="col-md-12 col-sm-12 col-lg-12" style="padding-bottom: 0px;">
                <v-textarea v-model="product.description" id="desc" class="main__add__desc-input" name="text" placeholder="Введите описание товара" solo :rules="[rules.required]"></v-textarea>
              </div>
              <div class="col-md-12 col-sm-12 col-lg-12" style="padding: 0px 12px;">
                <label class="main__add__label" for="link">Ссылка на изображение товара</label><i class="fas fa-circle fa-xs" style="color:#FF8484;margin-left:10px;"></i>
              </div>
              <div class="col-md-12 col-sm-12 col-lg-12" style="padding-bottom: 0px;">
                <v-text-field v-model="product.imgUrl" :rules="[rules.required]" class="main__add__link-input" id="link" placeholder="Введите ссылку" required solo></v-text-field>
              </div>
              <div class="col-md-12 col-sm-12 col-lg-12" style="padding: 0px 12px;">
                <label for="price" class="main__add__label">Цена товара</label><i class="fas fa-circle fa-xs" style="color:#FF8484;margin-left:10px;"></i>
              </div>
              <div class="col-md-12 col-sm-12 col-lg-12" style="padding-bottom: 0px;">
                <v-text-field v-model="product.price" class="main__add__price-input" :rules="[rules.required]" id="price" placeholder="Введите цену" required solo></v-text-field>
              </div>
              <div class="col-md-12 col-sm-12 col-lg-12">
                <v-btn class="main__add__btn" :disabled="!isValid" color="#13c27c" @click="addProduct">Добавить товар</v-btn>
              </div>
            </div>
          </form>
        </div>
        <div class="col-md-4 col-sm-4 col-lg-3" v-for="(item, index) in items" :key="index" style="padding-top: 0px;">
          <div class="main__card">
            <div class="main__card__img">
              <img :src="item.imgUrl" alt="Img"><i class="main__card__img__icon far fa-trash-alt" style="color:#fff;" @click="deleteProduct(item)"></i>
            </div>
            <div class="main__card__body">
              <div class="main__card__body__title"><h4>{{item.title}}</h4></div>
              <div class="main__card__body__text"><p>{{item.description}}</p></div>
              <div class="main__card__body__price"><h5>{{item.price}} руб</h5></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Loader from '../components/Loader.vue';
  export default {
    name: 'Home',
    components: { 
      Loader 
    },
    data() {
      return {
        items: [
          {
            title: "Наименование товара1",
            description: "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк",
            price: 5000,
            imgUrl: "/camera.svg"
          },
          {
            title: "Наименование товара2",
            description: "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк",
            price: 10000,
            imgUrl: "/camera.svg"
          },
          {
            title: "Наименование товара3",
            description: "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк",
            price: 1000,
            imgUrl: "/camera.svg"
          },
          {
            title: "Наименование товара4",
            description: "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк",
            price: 2500,
            imgUrl: "/camera.svg"
          },
          {
            title: "Наименование товара5",
            description: "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк",
            price: 3000,
            imgUrl: "/camera.svg"
          }
        ],
        sorts: [
          {
            name: "Возрастание по цене",
            id: 1
          },
          {
            name: "Убывание по цене",
            id: 2
          },
          {
            name: "По наименованию",
            id: 3
          }
        ],
        rules: {
            required: value => !!value || "Обязательно",
        },
        openDialog: false,
        product: {
          title: "",
          description: "",
          imgUrl: "",
          price: null
        },
        isLoading: false,
      };
    },
    computed: {
      isValid() {
        return this.product.title && this.product.description && this.product.imgUrl && this.product.price;
      }
    },
    methods: {
      onChange(item) {
        switch (item) {
            case 1:
              this.items = this.items.sort((a, b) => parseFloat(a.price) - parseFloat(b.price));
              break;
            case 2:
               this.items = this.items.sort((a, b) => parseFloat(b.price) - parseFloat(a.price));
              break;
            case 3:
              this.items = this.items.sort((a, b) => (a.title || "").toString().localeCompare((b.title || "").toString()));
              break;
         }
      },
      openAddModal() {
        this.isLoading = true;
        setTimeout(() => {
          this.isLoading = false;
          this.openDialog = !this.openDialog;

        },900)
      },
      addProduct() {
        
        this.isLoading = true;
        setTimeout(() => {
          this.items.push(this.product);
          this.openDialog = false;
          this.product = {};
          this.isLoading = false;
        },900)
      },
      deleteProduct(item) {
        this.isLoading = true;
        setTimeout(() => {
          this.items = this.items.filter(i => i.title != item.title);
          this.isLoading = false;
        },900)
        
      }
    }
  }
</script>
<style lang="scss" scoped>
/*box-shadow: 0px 5px 15px rgba(7, 8, 8, 0.5);*/
.main {
  background: #E5E5E5;
  min-height: 100vh;
  padding: 20px;
  &__card {
    width: 100%;
    background: #fff;
    border-radius: 10px;
    height: 420px;
    box-shadow: 0px 2px 3px rgba(7, 8, 8, 0.5);
    margin-bottom: 20px;
    transform: scale(0.9);
  transition: all 0.2s ease-in-out;
  will-change: transform;
    &__img {
      position:relative;
      & img {
        width: 100%;
        height: 200px;
        object-fit: cover;
      }
      &__icon {
        position: absolute;
        top: -10px;
        right: -10px;
        border: 1px solid #FF8484;
        background: #FF8484;
        border-radius: 10px;
        padding:7px;
        cursor: pointer;
        visibility: hidden;
      }
    }
    &__body {
      padding: 20px;
      &__title {
        margin-bottom: 10px;
      }
      &__text {
        -webkit-box-orient: vertical;
        display: -webkit-box;
        -webkit-line-clamp: 5;
        overflow-y: hidden;
        text-overflow: ellipsis;
        height: 120px;
      }
      &__price {}
    }
    &:hover {
      box-shadow: 0px 5px 15px rgba(7, 8, 8, 0.5);
      transform: translateY(1rem);
      transition: transform 1.5s 0s, opacity 1.25s 0s;
      & i {
        visibility: visible;
      }
    }
  }
  &__addbtn {
    border: 1px solid #3bbfb6;
    background: #3bbfb6;
    padding: 10px 20px;
    border-radius: 10px;
    color: #fff;
    letter-spacing: 1px;
  }
  &__sort {
    max-width: 200px;
    float: right;
  }
  &__add {
    background: #FFFEFB;
    border-radius: 10px;
    padding: 20px 30px;
    margin: 0px 10px;
    width: 95%;
    &__label {
      font-size: 15px;
      line-height: 13px;
      letter-spacing: -0.02em;
      color: #49485E;
    }
    &__name-input {
      width: 100%;
      padding: 10px;
    }
    &__link-input {
      width: 100%;
      padding: 10px;
    }
    &__price-input {
      width: 100%;
      padding: 10px;
    }
    &__desc-input {
      border-radius: 4px;
      padding: 10px;
    }
    &__btn {
      color: #fff !important;
      border-radius: 10px;
      width: 100%;
      height: 42px;
      font-weight: 700;
      text-transform: capitalize;
    }
    
  }
  
}

</style>
