<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item, index) in goods"
            class="menu-item menu-list-hook" :key="item.id"
            :class="{'current': currentIndex === index}"
            @click="selectMenu(index)">
          <span class="text border-1px">
            <span v-show="item.type > 0"
                  class="icon"
                  :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="food-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="item in goods" :key="item.id" class="food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" :key="food.id" class="food-item">
              <div class="icon">
                <img :src="food.icon" width="57" height="57">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥<span class="number">{{food.price}}</span></span>
                  <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import axios from 'axios'
  import BScroll from 'better-scroll'

  const ERR_OK = 0

  export default {
    data() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]

          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            this._followScroll(i)
            return i
          }
        }
        return 0
      }
    },
    created() {
      axios.get('/api/goods').then(res => {
        if (res.data.errno === ERR_OK) {
          this.goods = res.data.data
          this.$nextTick(() => {
            this._initScroll()
            this._calcHeight()
          })
        }
      })
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    methods: {
      selectMenu(index) {
        let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook')
        let el = foodList[index]
        this.foodScroll.scrollToElement(el, 300)
      },
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        })
        this.foodScroll = new BScroll(this.$refs.foodWrapper, {
          probeType: 3
        })
        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      _calcHeight() {
        let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook')
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
      },
      _followScroll(index) {
        let menuList = this.$refs.menuWrapper.getElementsByClassName('menu-list-hook')
        let el = menuList[index]
        this.menuScroll.scrollToElement(el, 300, 0, -300)
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
  .goods {
    display: flex;
    position: absolute;
    top: 174px;
    bottom: 46px;
    width: 100%;
    overflow: hidden;
    .menu-wrapper {
      flex: 0 0 80px;
      width: 80px;
      background-color: #f3f5f7;
      font-size: 0;
      .menu-item {
        display: table;
        line-height: 14px;
        width: 56px;
        height: 54px;
        padding: 0 12px;
        &.current {
          position: relative;
          margin-top: -1px;
          z-index: 10;
          background-color: #fff;
          font-weight: 700;
          .text {
            border: none;
          }
        }
        .icon {
          display: inline-block;
          width: 12px;
          height: 12px;
          background-size: cover;
          margin-left: 2px;
          background-repeat: no-repeat;
          vertical-align: top;
          &.decrease {
            @include bg-img('decrease_3');
          }
          &.discount {
            @include bg-img('discount_3');
          }
          &.guarantee {
            @include bg-img('guarantee_3');
          }
          &.invoice {
            @include bg-img('invoice_3');
          }
          &.special {
            @include bg-img('special_3');
          }
        }
        .text {
          display: table-cell;
          vertical-align: middle;
          font-size: 12px;
          @include border-1px(rgba(7, 17, 27, 0.1), bottom);
        }
      }
    }
    .food-wrapper {
      flex: 1;
      .title {
        padding-left: 14px;
        line-height: 26px;
        border-left: 2px solid #d9dde1;
        font-size: 12px;
        background-color: #f3f5f7;
        color: rgb(147, 153, 159);
      }
      .food-item {
        display: flex;
        margin: 0 18px;
        padding: 18px 0;
        & + .food-item {
          @include border-1px(rgba(7, 17, 27, 0.1), top);
        }
        .icon {
          flex: 0 0 57px;
          margin-right: 10px;
        }
        .content {
          flex: 1;
          .name {
            font-size: 14px;
            padding-top: 2px;
          }
          .desc {
            margin: 8px 0;
            color: rgb(147, 153, 159);
            font-size: 10px;
          }
          .extra {
            color: rgb(147, 153, 159);
            font-size: 0;
            .count, .rating {
              font-size: 10px;
            }
            .count {
              margin-right: 12px;
            }
          }
          .price {
            line-height: 24px;
            font-weright: 700;
            font-size: 0;
            .now {
              font-size: 10px;
              color: rgb(240, 20, 20);
              .number {
                font-size: 14px;
              }
            }
            .old {
              font-size: 10px;
              color: rgb(147, 153, 159);
              text-decoration: line-through;
              margin-left: 8px;
            }
          }
        }
      }
    }
  }
</style>
