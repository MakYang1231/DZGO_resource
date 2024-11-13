<template>
    <NuxtLayout name="total-rank-list">
        <div class="main" v-if="currentDataSet === 1">
            <ul class="list list-group font-weight-bold list-header">
              <li class="list-item list-group-item">
                <div :class="{'flex2_center': item === '彈珠玩家', 'flex1_center': item !== '彈珠玩家'}" v-for="(item, index) in title_2" :key="index"> ${ item } </div>
              </li>
            </ul>
            <div v-for="(group, groupIndex) in sortedItems_All" :key="groupIndex" class="main-Carousel" v-show="groupIndex === currentIndex">
            <transition-group name="list" tag="ul" mode="out-in" class="list list-group">  
              <li class="list-item list-group-item" v-for="(item, index) in group" :class="`list-item-animation list-item-${index+1}`" :key="item.user_id" :style="{ order: index+1, animationDelay: `${index * 0.2}s` }">
                    <div class="d-flex justify-content-center flex1_center">
                      <div class="number">${ (groupIndex * groupSize) + index+1 }</div>
                    </div>
                    <div class="picture flex1_center">
                      <NuxtImg class="lazyload" itemprop="image" :src="`${ item.user_picture }`" :data-src="`${ item.user_picture }`" /> 
                  </div>
                  <div class="info flex2_center">
                    <div class="name">${ item.user_name }</div>
                  </div>
                  <div class="info flex1_center">
                    <div class="point">
                        <countTo :startVal="0" :endVal="item.user_accumulated_fight_point" :duration="2000" separator="," class="me-2 size-bg"/>
                        <img v-if="item.user_rank_up_indicator === true" class="lazyload" itemprop="image" src="/images/ranking/up.png" data-src="images/ranking/up.png" />
                        <!-- ${ formatNumber(item.user_fight_point) } -->
                    </div>
                  </div>
                  <div class="icon flex1_center">
                    <img v-if="item.user_medal === 2" class="lazyload" itemprop="image" src="/images/ranking/medal_silver.png" data-src="/images/ranking/medal_silver.png" /> 
                    <img v-else-if="item.user_medal === 3" class="lazyload" itemprop="image" src="/images/ranking/medal_gold.png" data-src="images/ranking/medal_gold.png" /> 
                    <img v-else-if="item.user_medal === 4" class="lazyload" itemprop="image" src="/images/ranking/medal_diamond.png" data-src="images/ranking/medal_diamond.png" /> 
                    <img v-else class="lazyload" itemprop="image" src="/images/ranking/medal_copper.png" data-src="images/ranking/medal_copper.png"/>                     
                  </div>                  
                  <div class="title flex1_center">
                        <div class="title_gray_50">${ Math.floor(item.user_medal_PR * 100) / 100 }%</div>
                  </div>
              </li>
            </transition-group>
            </div>
        </div>        
    </NuxtLayout>   
  </template>
  
  <script setup lang="ts">
    definePageMeta({
        layout: false
    });

    const router = useRouter().currentRoute.value;
    const subbranch = router.params.subbranch;

    const items_2 = ref([]); 
    const title_2 = ['全台排名', '頭像', '彈珠玩家', '戰力值', '排位', '分佈'];

    //輪播 init
    let intervalId: ReturnType<typeof setInterval>;
    const currentIndex = ref(0);
    const currentDataSet = ref(1); // 1 代表資料1，2 代表資料2
    const groupSize = 10; // 設定每幾筆資料為一組

    const allList_speed = 5*1000; // 輪播速度 (ms)


    const sortedItems_All = computed(() => { 
        const result = [];
        items_2.value = items_2.value.slice().sort((a, b) => b.user_accumulated_fight_point - a.user_accumulated_fight_point);
        for(let i = 0; i < items_2.value.length; i += groupSize) {
            result.push(items_2.value.slice(i, i + groupSize));
        }
        return result;
    });    
  
  async function fetchData() {
  try {
    const response_All = await $fetch('https://isnmk.com/point/accumulated_ranking_api');
    updateRankings_All(response_All.all);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
  }

  function updateRankings_All(newData) {
        items_2.value = newData;
  }

  function startCarousel(speed: number) {
    intervalId = setInterval(() => {
        // 確定當前資料集
        const currentData = sortedItems_All;
        
        // 切換到下一組
        currentIndex.value = (currentIndex.value + 1) % currentData.value.length;
        // 如果當前資料集輪播完畢且沒有在上面進行過資料集切換，則切換到另一個資料集
    }, speed);
  }  

  onMounted(() => {
    fetchData(); // 初次加载时获取数据
    startCarousel(allList_speed); // 開始輪播
  });
  </script>
  
  
  <style lang="scss" scoped>
  .list-item {
      display: block;
      transition: all 1s ease;
      position: relative;
    }
    
    /* 過渡效果 */
    .list-move {
      transition: all 1s ease-in-out;;
    }
    .list-enter-active, .list-leave-active {
      transition: all 2s ease-in-out;;
    }    
    
    .list-enter-from, .list-leave-to {
      opacity: 0;
      transform: translateY(30px);
    }
    .list-enter-to, .list-leave-from {
      opacity: 1;
      transform: translateY(0px);
    }    
  
    .font-weight-bold {
      font-weight: 700;
    }
    .main {
        width: 100%;
        background-color: #f1f1f1;
        color: #252526;
        padding-bottom: 1rem;
        padding-top: 3rem;
        //background: url(../images/ranking/bg_3.png) center / cover no-repeat fixed;
  
        .storeItem {
            margin: .5rem 0;
            display: flex;
            align-items: center;
            justify-content: space-evenly;
  
            .item {
                font-size: 1.4rem;
                font-weight: 300;
            }
            .active {
                font-size: 1.8rem;
                color: #e21111;
            }
        }
  
        .list {
            background-color: var(--WEB_main_color);
            margin: 0 auto;
            width: 95%;
  
            .list-item {
                display: flex;
                justify-content: space-around;
                align-items: center;
                padding-top: 0.1rem;
                padding-bottom: 0.1rem;
                //background-color: inherit;
  
                .flex1_center {
                  flex: 1;
                  text-align: center;
                  font-size: .8rem;
                }
                .flex2_center {
                  flex: 2;
                  text-align: center;
                  font-size: .8rem;
                }              
  
                .number {
                    position: relative;
                    //background-color: #fbe7e9;
                    width: 1rem;
                    height: calc(1rem * cos(30deg) * 2);
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    font-size: 1rem;
                }
                .--number::after {
                    position: absolute;
                    content: "";
                    left: 0;
                    top: 0;
                    width: 0;
                    height: 0;
                    border: .5rem solid transparent;
                    border-top-width: calc((1rem * cos(30deg) * 2) / 2);
                    border-bottom-width: calc((1rem * cos(30deg) * 2) / 2);
                    border-right-color: #fbe7e9;
                    transform: translate(-100%);
                }
                .--number::before {
                    position: absolute;
                    content: "";
                    left: 0;
                    top: 0;
                    width: 0;
                    height: 0;
                    border: .5rem solid transparent;
                    border-top-width: calc((1rem * cos(30deg) * 2) / 2);
                    border-bottom-width: calc((1rem * cos(30deg) * 2) / 2);
                    border-left-color: #fbe7e9;
                    transform: translate(100%);                    
                }
                .picture {
                    img{
                        width: 3.5rem;
                        height: 3.5rem;
                        border-radius: 50%;
                    }
                }
  
                .info {
                    .name {
                        font-size: 1rem;
                        display: -webkit-box;
                        overflow: hidden;
                        -webkit-line-clamp: 1;
                        -webkit-box-orient: vertical;
                    }
                    .point{
                        display: flex;
                        align-items: center;
                        justify-content: center;
                        color: #ee4d2d;
                        font-size: 1.4rem;    
  
                        .text{
                            color: #9d9d9d;
                            font-size: 1.4rem;
                            margin-right: .5rem
                        }
                        img {
                            width: 1.5rem;
                            height: 1.5rem;
                        }
                    }
                    .size-bg {
                      font-size: 1rem;
                    }
                }
  
                .title {
                    .title_default {
                        color: snow;
                        padding: .25rem;
                        font-size: 2.0rem;
                        border-radius: 5px;
                        width: fit-content;
                        margin: 0 auto;
                    }
                    .title_three {
                        background: linear-gradient(to bottom right, #7f7cbc, #dde2e8);
                    }
                    .title_two {
                        background: linear-gradient(to bottom right, #132360, #4f84d7);
                    }
                    .title_one {
                        background: linear-gradient(to bottom right, #d3b66f, #8a50d0);
                    }
                    .title_copper {
                        background: linear-gradient(to bottom right, #edaa00, #873324);
                    }
                    .title_silver {
                        background: linear-gradient(to bottom right, #c5c5c5, #606060);
                    }
                    .title_gold {
                        background: linear-gradient(to bottom right, #fff347, #ff8c56);
                    }
                    .title_diamond {
                        background: linear-gradient(to bottom right, #2f25ea, #c6c8e4);
                    }
                    .title_gray_25 {
                        color: #BFBFBF;
                    }
                    .title_gray_50 {
                        color: #A3A3A3;
                    }                    
                    .title_gray_75 {
                        color: #5B5B5B;
                    }                    
                    .title_dark {
                        color: #000000;
                    }
                }
  
                .icon img{
                    width: 3.5rem;
                    height: 3.5rem;
                }
            }
        }
  
        .list-header{
          position: sticky;
          top: 0;
          z-index: 1024;
        }

        .main-Carousel {
            .list {
                .list-item-animation {
                    opacity: 0;
                    transform: translateY(100%); /* 初始位置在畫面右邊 */
                    animation: slideIn 2.4s forwards ease-out;
                }
            }    
        }
    }

    .list-item-focus {
        border-image: url(../images/ranking/border.png) 190 / 30px / 5px round;
        z-index: 999999;
        border-width: 30px;
    }
    .enter-model {
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: space-evenly;
        align-items: center;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.9); /* 半透明背景 */  
        align-items: center;
        z-index: 99999;

        div {
            display: flex;
            flex-direction: column;
            flex-wrap: nowrap;
            justify-content: space-evenly;
            align-items: center;   
            width: 100%;         
        }

        .model_img {
            width: 80%;
            height: 50%;
        }
    }
/* 定義從右邊淡入的動畫 */
@keyframes slideIn {
  to {
    opacity: 1;
    transform: translateY(0px);
  }
}    
  </style>