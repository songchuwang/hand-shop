<template>
  <div class="container">
    <div class="content">
      <div class="fore">
        <h4>个人中心</h4>
        <div @click="showPage('1')" class="toPage">个人闲置</div>
        <div @click="showPage('2')" class="toPage">我的订单</div>
        <div @click="showPage('3')" class="toPage">忘记密码</div>
      </div>
      <div class="page">
        <div v-if="isShowPage == 1" class="page-switch page-one">
          <section class="header">
            <div class="header-img">
              <div class="img">
                <img src="../assets/img/WechatIMG139.jpeg" alt="">
              </div>
              <div class="name">{{this.user}}</div>
            </div>
            <div class="header-content">
              <div class="header">我的管理</div>
              <div></div>
            </div>
          </section>

          <section class="content">
            <div class="surplus">我的闲置</div>
            <div class="select">
              <el-tabs type="border-card">
                <el-tab-pane label="已发布">
                  <div class="container" v-for="(item, index) in this.my_surplus" :key="index">
                    <div class="order-header">
                      {{item.gname}}
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div style="padding-right:20px" class="footer">
                      <div style="background: #ff3434;color:#fff" @click="cancelRelease(item)" class="cancel-order">取消发布</div>
                    </div>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="待发货">
                  <div class="container" v-for="(item, index) in this.releaser_deliveries" :key="index">
                    <div class="order-header">
                      {{item.gname}}
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div class="footer">
                      <!-- <div @click="cancelOrder" class="cancel-order">取消发布</div> -->
                      <div style="margin-right:20px;color:red;">买家已付款，请尽快发货</div>
                      <div @click="deliverGoods(item)" class="payoff">发货</div>
                    </div>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="待收货">
                  <div class="container" v-for="(item, index) in this.wait_release" :key="index">
                    <div class="order-header">
                      {{item.gname}}
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div class="footer">
                      <div style="margin-right:20px;color:red;">您已发货，等待买家确认收货</div>
                      <div @click="payoff" class="payoff">已发货</div>
                    </div>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="已完成">
                  <div class="container" v-for="(item, index) in this.release_complete" :key="index">
                    <div class="order-header">
                      {{item.gname}}
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div class="footer" style="padding-right:40px">
                      <div style="color:red;">已完成订单</div>
                      <!-- <div @click="payoff" class="payoff">删除订单</div> -->
                    </div>
                  </div>
                </el-tab-pane>
              </el-tabs>
            </div>
            <div class="content-inner"></div>
          </section>

        </div>
        <div v-if="isShowPage == 2" class="page-switch page-two">
          <div class="content">
            <div class="header">
              我的订单
            </div>
            <div class="header-nav">
              <el-tabs style="width:1000px" type="border-card" v-model="activeName" @tab-click="handleClick">
                <el-tab-pane style="width:250px" label="全部" name="first">
                  <div class="container" v-for="(item, index) in this.all_orders" :key="index">
                    <div class="order-header">
                      {{item.gname}}
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div class="footer">
                      <div @click="cancelOrder(item)" class="cancel-order">取消订单</div>
                      <div @click="payoff" class="payoff">去付款</div>
                    </div>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="待付款" name="second">
                  <div class="container" v-for="(item, index) in this.order_infor" :key="index">
                    <div class="order-header">
                      {{item.gname}}
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div class="footer">
                      <div @click="cancelOrder(item)" class="cancel-order">取消订单</div>
                      <div @click="payoff(item)" class="payoff">付款</div>
                    </div>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="待发货" name="third">
                  <div class="container" v-for="(item, index) in this.deliveries" :key="index">
                    <div class="order-header">
                      <div class="gname">
                        {{item.gname}}
                      </div>
                      <div class="wait">待发货</div>
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div class="footer">
                      <div @click="remind" class="cancel-order">催发货</div>
                      <div class="payoff">已付款</div>
                    </div>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="待收货" name="fourth">
                  <div class="container" v-for="(item, index) in this.confirm_receipt" :key="index">
                    <div class="order-header">
                      {{item.gname}}
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div class="footer">
                      <!-- <div @click="cancelOrder" class="cancel-order">取消订单</div> -->
                      <div @click="confirmReceipt(item)" class="payoff">确认收货</div>
                    </div>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="已完成" name="five">
                  <div class="container" v-for="(item, index) in this.complete" :key="index">
                    <div class="order-header">
                      {{item.gname}}
                    </div>
                    <div class="content">
                      <img :src="`data:image/jpg;base64,${item.imgs[0].url}`" alt="">
                      <div class="desc">{{item.desc}}</div>
                      <div class="price">￥{{item.sale_price}}</div>
                    </div>
                    <div class="footer">
                      <!-- <div @click="cancelOrder" class="cancel-order">取消订单</div> -->
                      <div style="margin-right:20px;color:red;">订单已完成</div>
                      <!-- <div @click="confirmReceipt(item)" class="payoff">确认收货</div> -->
                    </div>
                  </div>
                </el-tab-pane>
              </el-tabs>
            </div>
            <div>

            </div>
          </div>
        </div>
        <div v-if="isShowPage == 3" class="page-switch page-three">
          <div class="content">
            <div class="header">
              忘记密码
            </div>
            <div class="content-detail">
              <p>请联系客服 400-612-2333</p>
              <p>或发送邮件至 lidongqing@paipai.com <span class="round-btn">发送邮件</span></p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        isShowPage: 1,
        activeName: 'second',
        all_orders: [],
        goodsInfor: '',
        wait_release:[],
        confirm_receipt: [],
        orders: '',
        releaser_deliveries: [],
        deliveries: '',
        order_infor: [],
        my_surplus: '',
        complete:[],
        release_complete:[],
        user: ''
      }
    },
    created() {
      this.getOrder()
      this.mySurplus()

    },
    methods: {
      cancelRelease(item) {
        console.log(item._id);
        
        this.$axios.post('/goods/searchComplete',{
          _id:item._id
        }).then(res=>{
          console.log(res.data.code);
          
          if(res.data.code == 0){
            this.$message.error('该商品已完成交易，无法取消')
          }else if(res.data.code == -1){
            this.$message.success("已取消发布")
            this.$axios.post('/goods/removeGoods',{
              _id:item._id
            }).then(res2 => {
              console.log(res2);
              if(res2.status == 200){
                window.location.reload()
              }
              
            })
          }
        })
        // console.log(item);
        
      },
      confirmReceipt(item) {
        this.$axios.post('/goods/toComplete',item).then(res=>{
          if(res.status == 200){
            
            this.$message.success('收货成功')
            setTimeout(()=>{
              window.location.reload()
            },1000)
          }
          
        })
        
      },
      deliverGoods(item) {
        // console.log(item);
        
        this.$axios.post('/goods/addWait',item).then(res=>{
          // console.log(res.data.data);
          if(res.status == 200) {
            this.wait_release = res.data.data
            console.log(this.wait_release);
            
            this.$message.success('发货成功，等待买家收货')
          }
        })
        
      },
      // 我发布的商品
      mySurplus() {
        this.$axios.post('goods/getGoods').then(res => {
          this.my_surplus = res.data.data.filter((item) => {
            return this.$store.state.geo.userId == item.publisher
          })
        })

      },
      remind() {
        this.$message.success('已提醒卖家')
      },

      payoff(item) {
        // console.log(item);

        this.$message.success('付款成功')
        this.$axios.post('/goods/addDelivery', item).then(res => {
          if (res.status == 200) {
            this.$message.success('付款成功')
            setTimeout(() => {
              window.location.reload()
            }, 1000)
          }

        })
        // if(this.ord)
      },
      receiving() {
        this.$message.success('收货成功')
      },
      cancelOrder(item) {
        console.log(item);
        
        this.$axios.post('/goods/cancelOrder', item).then(res=>{
          if(res.status == 200){
            this.$message.success('订单已取消')
          }
        })
        
      },
      showPage(index) {
        this.isShowPage = index;
      },
      handleClick(tab, event) {},
      getOrder() {
        let id = this.$store.state.geo.userId
        this.$axios.post('/goods/getorder', {
          id
        }).then(res => {
          this.order_infor = res.data.data
        })
      }
    },
    mounted() {
      this.$axios.post('/goods/getGoodInfor', {
        _id: this.$route.query.buy
      }).then(res => {
        this.goodsInfor = res.data.data
        this.isShowPage = 2;
      })
      this.$axios.get('/users/getUser').then(res => {
        this.user = decodeURIComponent(res.data.user);
      });
      console.log(this.$store.state.geo.userId);
      

      this.$axios.post('/goods/getAllDelivery', {}).then(res => {
        console.log('delevery',res.data.data);
        this.releaser_deliveries = res.data.data.filter((item)=>{
          return item.publisher == this.$store.state.geo.userId
        })
        
      })
      


      this.$axios.post('/goods/getDelivery', {
        id: this.$store.state.geo.userId
      }).then(res => {

        this.deliveries = res.data.data;
        this.all_orders = this.deliveries.concat(this.order_infor);
      })

      this.$axios.post('/goods/getWaitGoods').then(res=>{
        this.confirm_receipt = res.data.data.filter((item)=>{
          return item.id == this.$store.state.geo.userId
        });
      })
      // confirm_receipt
      this.$axios.post('/goods/getWaitGoods').then(res=>{
        this.wait_release = res.data.data.filter((item)=>{
          return item.publisher == this.$store.state.geo.userId
        });
      })
      this.$axios.post('/goods/getComplete').then(res=>{
        this.complete = res.data.data.filter((item)=>{
          return item.id == this.$store.state.geo.userId
        });
      })
      // release_complete
      this.$axios.post('/goods/getComplete').then(res=>{
        this.release_complete = res.data.data.filter((item)=>{
          return item.publisher == this.$store.state.geo.userId
        });
      })

      this.mySurplus();

    },
  }

</script>

<style lang="scss" scoped>
  .container {
    .content {
      width: 1190px;
      position: relative;

      .fore {
        width: 170px;
        height: 160px;
        box-sizing: border-box;
        padding: 15px 20px;
        position: absolute;
        top: -20px;
        margin-right: 20px;
        background: #fff;

        h4 {
          font-size: 16px;
          font-weight: bolder;
          margin-bottom: 10px;
        }

        div {
          width: 130px;
          height: 32px;
          line-height: 32px;
          font-size: 14px;
          cursor: pointer;

          &:hover {
            color: #01c30b;
          }
        }
      }

      .page {
        width: 1000px;
        margin-left: 190px;
        margin-top: 20px;
        position: relative;

        .page-one {
          width: 100%;

          .header {
            width: 100%;
            height: 250px;
            box-sizing: border-box;
            display: flex;
            flex-direction: row;
            justify-content: flex-start;
            align-items: center;

            .header-img {
              width: 250px;
              height: 250px;
              background: #daccb1;
              position: relative;

              .img {
                width: 56px;
                height: 56px;
                position: absolute;
                top: 38%;
                left: 50%;
                transform: translate(-50%, -50%);
                border-radius: 50%;
                border: 4px solid #fff;
                display: flex;
                flex-direction: row;
                justify-content: center;
                align-items: center;

                img {
                  width: 56px;
                  height: 56px;
                  border-radius: 50%;
                }
              }

              .name {
                font-size: 18px;
                font-weight: 700;
                color: #fff;
                position: absolute;
                top: 62%;
                left: 50%;
                transform: translate(-50%, -50%);
              }
            }

            .header-content {
              width: 750px;
              height: 225px;
              background: #fff;
              display: flex;
              flex-direction: column;
              justify-content: flex-start;

              .header {
                width: 100%;
                height: 55px;
                line-height: 55px;
                box-sizing: border-box;
                padding-left: 20px;
                border-bottom: 1px solid #f0f3ef;
              }
            }
          }

          .content {
            width: 1000px;
            background: #fff;

            .container {
              width: 1000px;
              display: flex;
              flex-direction: column;
              justify-content: flex-start;
              box-sizing: border-box;
              // border-bottom: 1px dashed #dfdfdf;
            }

            .order-header {
              width: 100%;
              height: 50px;
              line-height: 50px;
              display: flex;
              flex-direction: row;
              justify-content: space-between;
              border-bottom: 1px dashed #dfdfdf;
              padding-left: 12px;
            }

            .content {
              width: 100%;
              height: 100px;
              box-sizing: border-box;
              padding: 12px 0;
              // line-height: 76px;
              display: flex;
              flex-direction: row;
              justify-content: flex-start;
              align-items: center;
              padding-left: 12px;

              border-bottom: 1px dashed #dfdfdf;

              img{
                width: 80px;
                height: 80px;
              }
              // align-items: center;
              .desc {
                width: 800px;
                height: 100%;
                display: flex;
                flex-direction: column;
                justify-content: flex-start;
                box-sizing: border-box;
                padding-left: 10px;
              }

              .price {
                font-size: 14px;
                height: 100%;
                font-family: jdZhengHeiRegular;
                font-weight: 600;
              }
            }

            .footer {
              width: 100%;
              height: 72px;
              padding: 17px 0;
              line-height: 38px;
              box-sizing: border-box;
              display: flex;
              flex-direction: row;
              justify-content: flex-end;
              padding-left: 12px;

              .cancel-order {
                width: 90px;
                height: 38px;
                line-height: 38px;
                background: #fff;
                border: 1px solid #d9d9d9;
                box-sizing: border-box;
                color: #333333;
                text-align: center;
                border-radius: 20px;
                margin-right: 20px;
                cursor: pointer;
              }

              .payoff {
                width: 90px;
                height: 38px;
                line-height: 38px;
                background: #ff3434;
                cursor: pointer;
                box-sizing: border-box;
                color: #fff;
                text-align: center;
                border-radius: 20px;
                margin-right: 30px;
              }
            }

            .surplus {
              width: 100%;
              box-sizing: border-box;
              height: 55px;
              line-height: 55px;
              padding-left: 20px;
              border-bottom: 1px solid #f0f3ef;
              font-size: 16px;
            }

            // .select {
            //   display: flex;
            //   flex-direction: row;
            //   justify-content: space-between;
            //   box-sizing: border-box;

            //   >div {
            //     width: 50%;
            //     height: 55px;
            //     line-height: 55px;
            //     text-align: center;
            //     box-sizing: border-box;
            //     border-bottom: 1px solid #f0f3ef;

            //     &:first-child {
            //       border-right: 1px solid #f0f3ef;
            //     }
            //   }
            // }

            .content-inner {
              width: 100%;
            }

          }

        }

        .page-two {
          width: 100%;
          height: auto;
          background: #fff;

          .content {
            .header {
              width: 100%;
              height: 90px;
              padding: 20px 0 20px 20px;
              box-sizing: border-box;
              font-size: 22px;
              line-height: 50px;
            }

            .header-nav {
              display: flex;
              flex-direction: row;
              justify-content: space-between;
              width: 1000px;

              /deep/ .el-tabs__content {
                padding: 0;
                box-sizing: border-box;
              }

              .container {
                width: 1000px;
                display: flex;
                flex-direction: column;
                justify-content: flex-start;
                box-sizing: border-box;
                border-bottom: 1px dashed #dfdfdf;

                .order-header {
                  width: 100%;
                  height: 50px;
                  line-height: 50px;
                  display: flex;
                  flex-direction: row;
                  justify-content: space-between;
                  border-bottom: 1px solid #dfdfdf;
                  padding-left: 12px;

                  .wait {
                    margin-right: 80px;
                    color: goldenrod;
                  }
                }

                .content {
                  width: 100%;
                  height: 100px;
                  box-sizing: border-box;
                  padding: 12px 0;
                  // line-height: 76px;
                  display: flex;
                  flex-direction: row;
                  justify-content: flex-start;
                  align-items: center;
                  padding-left: 12px;

                  border-bottom: 1px solid #dfdfdf;

                  img {
                    width: 72px;
                    height: 72px;
                  }

                  .desc {
                    width: 800px;
                    height: 100%;
                    display: flex;
                    flex-direction: column;
                    justify-content: flex-start;
                    box-sizing: border-box;
                    padding-left: 10px;
                  }

                  .price {
                    font-size: 14px;
                    height: 100%;
                    font-family: jdZhengHeiRegular;
                    font-weight: 600;
                  }
                }

                .footer {
                  width: 100%;
                  height: 72px;
                  padding: 17px 0;
                  line-height: 38px;
                  box-sizing: border-box;
                  display: flex;
                  flex-direction: row;
                  justify-content: flex-end;
                  padding-left: 12px;

                  .cancel-order {
                    width: 90px;
                    height: 38px;
                    line-height: 38px;
                    background: #fff;
                    border: 1px solid #d9d9d9;
                    box-sizing: border-box;
                    color: #333333;
                    text-align: center;
                    border-radius: 20px;
                    margin-right: 20px;
                    cursor: pointer;
                  }

                  .payoff {
                    width: 90px;
                    height: 38px;
                    line-height: 38px;
                    background: #ff3434;
                    cursor: pointer;
                    box-sizing: border-box;
                    color: #fff;
                    text-align: center;
                    border-radius: 20px;
                    margin-right: 30px;
                  }
                }
              }

              ul {
                display: flex;
                flex-direction: row;
                justify-content: space-between;
                width: 1000px;

                li {
                  list-style: none;
                  width: 20%;
                  height: 66px;
                  line-height: 66px;
                  text-align: center;
                  font-size: 20px;
                  font-weight: 600;
                }
              }

            }
          }
        }

        .page-three {
          //   margin-top: -20px;
          position: relative;
          width: 1000px;
          height: 400px;
          background: #fff;

          .content {
            .header {
              width: 100%;
              height: 90px;
              padding: 20px 0 20px 20px;
              box-sizing: border-box;
              font-size: 22px;
              line-height: 50px;
            }

            .content-detail {
              box-sizing: border-box;
              padding-left: 20px;

              p {
                line-height: 28px;
                font-size: 14px;
                color: #666;

                .round-btn {
                  display: inline-block;
                  width: 64px;
                  height: 20px;
                  line-height: 20px;
                  font-size: 12px;
                  border: 1px solid #979797;
                  color: #979797;
                  text-align: center;
                  border-radius: 10px;
                  margin-left: 10px;
                  cursor: pointer;
                }
              }
            }
          }
        }
      }
    }
  }

</style>
