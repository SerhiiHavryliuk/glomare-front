<template>
  <NuxtLink :to="{name: 'categories-id', params: {id: item._id }}" :class="{'isGrid': grid}">
    <div class="img_wrap">
      <img :src="item.images[0] || 'https://www.svo.aero/assets/default-image.jpg'" alt="image" class="item_img">
    </div>

    <div class="item_content">
      <div class="title_wrap">
        <h4 class="item_title">
          {{ item.title }}
        </h4>
        <div>
          <p class="item_location">
            <v-icon size="16" color="var(--negative)">
              mdi-map-marker
            </v-icon>{{ item.location }}
          </p>
          <NuxtLink :to="{name: 'categories', params: {category: item.category}}" class="item_location">
            <v-icon size="16" color="var(--warning)">
              mdi-star-minus
            </v-icon>{{ item.category }}
          </NuxtLink>
        </div>
      </div>
      <div class="item_details">
        <p>Price:<span v-if="item.price > 0">{{ item.price }} UAH</span><span v-else>Free</span></p>
        <p>
          Rating:<span>{{ item.rating }} <v-icon
            size="20"
            color="yellow accent-4"
          >
            mdi-star
          </v-icon>
          </span>
        </p>
        <p v-show="isProfile">
          Term:<span>{{ term && term !== '' ? term || '0' : item.lease_term || '0' }} day(s)</span>
        </p>
        <p v-show="!isProfile">
          Status:<span>{{ item.status === 'unavailable' ? 'Rented' : 'In stock' }}</span>
        </p>
      </div>
      <NuxtLink :to="/profile/ +item.leaser_info.userId">
        <div class="item_seller">
          <img
            :src="item.leaser_info.avatar !== '' ?
              item.leaser_info.avatar :
              'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSaDVA_pu9U4m-YPEQY5c9v8nqJHzOPgmopaA&usqp=CAU'"
            alt="avatar"
          >
          <p>{{ item.leaser_info.nickname }}</p>
        </div>
      </NuxtLink>
      <slot />
    </div>
  </NuxtLink>
</template>

<script>
import { Vue, Component, Prop } from 'nuxt-property-decorator';
export default @Component({
  name: 'single-item'
})

class SingleItem extends Vue {
  @Prop({ type: Boolean, required: true }) grid;
  @Prop({ type: Object, required: true }) item;
  @Prop() term;
  isProfile = this.$route.path.includes('/rent');
}

</script>

<style lang="scss" scoped>
    .img_wrap {
      width: 100%;
    }

    .item_img {
      display: block;
      width: 100%;
      aspect-ratio: 3/2;
      object-fit: cover;
    }
    .item_location {
      display: block;
      margin-top: 3px;
      font-size: 14px;
      font-weight: 400;
      @media (max-width: 899px) {
        margin-bottom: 7px;
      }
      @media (max-width: 599px) {
        margin-top: 0;
      }
      i {
        margin-bottom: 5px;
      }
    }
    p.item_location {
      cursor: text;
    }

    .item_title {
      margin-top: 10px;
      margin-bottom: 0;
      font-size: 18px;
      height: 45px;
      @media screen and (max-width: 899px) {
        font-size: 16px;
        font-weight: 600;
      }
    }
    .item_details {
      display: flex;
      justify-content: space-between;
      border-top: 2px solid rgba($light, 0.4);
      border-bottom: 2px solid rgba($light, 0.4);
      @media (max-width: 899px) {
        border-top: 1px solid rgba($light, 0.4);
        border-bottom: 1px solid rgba($light, 0.4);

      }
      p {
        padding: 10px 3px 6px;
        margin: 0;
        font-size: 16px;
        @media (max-width: 899px) {
          padding: 0 3px 4px;
          font-size: 14px;
        }
        &:first-of-type span{
          font-size: 20px;
          color: $positive;
          @media (max-width: 899px) {
            font-size: 16px;
          }
      }
      }
      span {
        display: block;
        text-align: center;
        padding-top: 5px;
        color: $negative;
        font-weight: 600;
        @media (max-width: 899px) {
          font-size: 14px;
        }
        i {
          margin-bottom: 3px;
        }
      }
    }
    .item_seller {
      display: flex;
      align-items: center;
      margin-top: 6px;
      @media (max-width: 499px) {
        max-width: 140px;
      }
      img {
        width: 40px;
        height: 40px;
        object-fit: cover;
        border-radius: 50%;
        @media (max-width: 899px) {
          width: 35px;
          height: 35px;
        }
        @media (max-width: 499px) {
          width: 30px;
          height: 30px;
        }
      }
      p {
        margin: 0 0 0 5px;
        @media (max-width: 899px) {
          font-size: 16px;
        }
        @media (max-width: 499px) {
          font-size: 12px;
          line-height: 1.01;
        }
      }
    }

    .isGrid {
  display: flex;
  .item_content {
    margin-left: 20px;
      width: 60%
  }
  .img_wrap {
    width: 40%;
    height: 100%;
  }

  .item_img {
       height: 100%;
  }

  .title_wrap {
    display: flex;
    justify-content: space-between;
    @media (max-width: 599px) {
      flex-wrap: wrap;
    }
  }

  .item_title {
    @media (max-width: 599px) {
      margin-top: 0
    }
  }
  .item_details {
    display: block;
    border-bottom: none;
    span {
      display: inline-block;
      margin-left: 20px;
      padding: 0
    }
  }
}

</style>
