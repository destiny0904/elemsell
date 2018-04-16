<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keybord_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keybord_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade" :duration="500">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="detail-name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="detail-title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item, index) in seller.supports" :key="item.id">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="detail-title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <div class="content">
                {{seller.bulletin}}
              </div>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="closeDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>
<script type="text/ecmascript-6">
  import star from '../star/star'

  export default {
    data() {
      return {
        detailShow: false
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    methods: {
      showDetail() {
        this.detailShow = true
      },
      closeDetail() {
        this.detailShow = false
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    components: {
      star
    }
  }
</script>
<style lang="scss" rel="stylesheet/scss" scoped>
  .header {
    position: relative;
    color: #fff;
    background-color: rgba(7, 17, 27, 0.5);
    overflow: hidden;
    .content-wrapper {
      position: relative;
      padding: 24px 12px 18px 24px;
      font-size: 0;
      .avatar {
        display: inline-block;
        vertical-align: top;
      }
      .content {
        display: inline-block;
        margin-left: 16px;
        padding: 2px 0;
        .title {
          .brand {
            display: inline-block;
            width: 30px;
            height: 18px;
            @include bg-img('brand');
            background-size: cover;
            background-repeat: no-repeat;
            vertical-align: top;
          }
          .name {
            font-size: 16px;
            line-height: 18px;
            font-weight: bold;
            margin-left: 6px;
          }
        }
        .description {
          margin: 8px 0 10px 0;
          font-size: 12px;
        }
        .support {
          .icon {
            display: inline-block;
            width: 12px;
            height: 12px;
            background-size: cover;
            background-repeat: no-repeat;
            vertical-align: top;
            &.decrease {
              @include bg-img('decrease_1');
            }
            &.discount {
              @include bg-img('discount_1');
            }
            &.guarantee {
              @include bg-img('guarantee_1');
            }
            &.invoice {
              @include bg-img('invoice_1');
            }
            &.special {
              @include bg-img('special_1');
            }
          }
          .text {
            font-size: 12px;
            margin-left: 4px;
          }
        }
      }
      .support-count {
        position: absolute;
        right: 12px;
        bottom: 14px;
        background-color: rgba(0, 0, 0, 0.2);
        box-sizing: border-box;
        padding: 7px 8px;
        height: 24px;
        border-radius: 14px;
        .count {
          line-height: 12px;
          font-size: 10px;
          vertical-align: bottom;
        }
        i {
          font-size: 10px;
          line-height: 12px;
          margin-left: 2px;
        }
      }
    }
    .bulletin-wrapper {
      position: relative;
      height: 28px;
      line-height: 28px;
      padding: 0 22px 0 12px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      background-color: rgba(7, 17, 27, 0.2);
      .bulletin-title {
        display: inline-block;
        width: 22px;
        height: 12px;
        @include bg-img('bulletin');
        background-size: cover;
        background-repeat: no-repeat;
        vertical-align: top;
        margin: 8px 0;
      }
      .bulletin-text {
        font-size: 10px;
        margin-left: 4px;
        vertical-align: top;
      }
      i {
        position: absolute;
        top: 8px;
        right: 12px;
        font-size: 10px;
      }
    }
    .background {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      filter: blur(10px);
      background-color: rgba(7, 17, 27, 0.5);
    }
    .detail {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 100;
      overflow: auto;
      background-color: rgba(7, 17, 27, 0.8);
      backdrop-filter: blur(10px);
      &.fade-enter-active, &.fade-leave-active {
        transition: all .5s;
      }
      &.fade-enter, &.fade-leave-to {
        opacity: 0;
        background-color: rgba(7, 17, 27, 0);
      }
      .detail-wrapper {
        min-height: 100%;
        width: 100%;
        .detail-main {
          padding: 64px 0;
          .detail-name {
            text-align: center;
            font-size: 16px;
            font-weight: 700;
          }
          .star-wrapper {
            margin-top: 18px;
            padding: 2px 0;
            text-align: center;
          }
          .detail-title {
            display: flex;
            width: 80%;
            margin: 28px auto 24px auto;
            .line {
              flex: 1;
              position: relative;
              top: -6px;
              border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            }
            .text {
              padding: 0 12px;
              font-size: 12px;
              font-weight: 700;
            }
          }
          .supports {
            width: 80%;
            margin: 0 auto;
            .support-item {
              padding: 0 12px;
              font-size: 0;
              .icon {
                display: inline-block;
                vertical-align: top;
                background-size: cover;
                background-repeat: no-repeat;
                width: 16px;
                height: 16px;
                &.decrease {
                  @include bg-img('decrease_2');
                }
                &.discount {
                  @include bg-img('discount_2');
                }
                &.guarantee {
                  @include bg-img('guarantee_2');
                }
                &.invoice {
                  @include bg-img('invoice_2');
                }
                &.special {
                  @include bg-img('special_2');
                }
              }
              .text {
                font-size: 12px;
                line-height: 16px;
                margin-left: 6px;
              }
            }
            .support-item + .support-item {
              margin-top: 12px;
            }
          }
          .bulletin {
            width: 80%;
            margin: 0 auto;
            .content {
              font-size: 12px;
              line-height: 24px;
              padding: 0 12px;
            }
          }
        }
      }
      .detail-close {
        position: relative;
        width: 32px;
        height: 32px;
        margin: -64px auto 0 auto;
        clear: both;
        text-align: center;
        i {
          font-size: 32px;
        }
      }
    }
  }
</style>
