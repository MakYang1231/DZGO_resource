<template>
  <NuxtLayout name="custom-layout">
      <div class="main">
          <div class="topImage">
              <img src="/images/main/1037089_0.jpg" alt="">
          </div>
          <div class="storeItem">
              <div class="item" :class="{ active: selectedStore === 'shilin' }" @click="fetchData('shilin')">士林店</div>
              <div class="item" :class="{ active: selectedStore === 'sanhe' }" @click="fetchData('sanhe')">三和店</div>
          </div>
          <ul class="list list-group font-weight-bold">
            <li class="list-item list-group-item">
              <div class="flex1_center">排名</div>
              <div class="flex1_center">頭像</div>
              <div class="flex2_center">人名/戰力值</div>
              <div class="flex1_center">財富值</div>
              <div class="flex1_center">衝榜</div>
            </li>
          </ul>
          <transition-group name="list" tag="ul" class="list list-group">
            <li class="list-item list-group-item" v-for="(item, index) in sortedItems" :key="item.user_id" :style="{ order: item.user_rank }">
                  <div class="d-flex justify-content-center flex1_center">
                    <div class="number">${index+1}</div>
                  </div>
                  <div class="picture flex1_center">
                    <NuxtImg class="lazyload" itemprop="image" :src="`${ item.user_picture }`" :data-src="`${ item.user_picture }`" /> 
                </div>
                <div class="info flex2_center">
                  <div class="name">${ item.user_name }</div>
                  <div class="point">
                      <div class="text">戰力值</div>
                      <countTo :startVal="0" :endVal="item.user_fight_point" :duration="2000" separator="," />
                      <!-- ${ formatNumber(item.user_fight_point) } -->
                  </div>
                </div>
                <div class="title flex1_center">
                      <div v-if="item.user_wealth_title === '薄有積蓄'" class="title_default title_silver">薄有積蓄</div>
                      <div v-else-if="item.user_wealth_title === '小有成就'" class="title_default title_gold">小有成就</div>
                      <div v-else-if="item.user_wealth_title === '生財有道'" class="title_default title_three">生財有道</div>
                      <div v-else class="title_default title_copper">白手起家</div>
                </div>
                <div class="icon flex1_center">
                  <NuxtImg v-if="item.user_rank_up_indicator === true" class="lazyload" itemprop="image" src="images/ranking/up.png" data-src="public/images/ranking/1_ranking.png" /> 
                </div>
            </li>
          </transition-group>
      </div>
  </NuxtLayout>
</template>

<script setup lang="ts">
  definePageMeta({
      layout: false
  });

  const items = ref([]);
  const selectedStore = ref('shilin')

  // Sorted list based on rank
  const sortedItems = computed(() => {
  return items.value.slice().sort((a, b) => b.user_fight_point - a.user_fight_point);
  });

async function fetchData(store) {
  selectedStore.value = store;
try {
  const response = await $fetch('https://isnmk.com/point/rank_api');
  if (store === 'shilin') {
    updateRankings(response.shilin);
  } else if (store === 'sanhe') {
    updateRankings(response.sanhe);
  }
} catch (error) {
  console.error('Error fetching data:', error);
}
}

function updateRankings(newData) {
if (JSON.stringify(newData) !== JSON.stringify(items.value)) {
  items.value = newData;
}
}

let wss;
onMounted(() => {
fetchData('shilin'); // 初次加载时获取数据

  // Connect to WebSocket server
  wss = new WebSocket('wss://isnmk.com/websocket/ranking/');
  console.log(wss);

  wss.onopen = function(event) {
    console.log('WebSocket connection successfully opened');
    // 你可以在这里执行连接成功后的操作
  };

  wss.onmessage = (event) => {
    console.log('WebSocket message received:', event.data);
    // Trigger API refresh when WebSocket message is received
    console.log(selectedStore.value);
    fetchData(selectedStore.value);
  };

  wss.onerror = (error) => {
    console.error('WebSocket Error:', error);
  };
});

onUnmounted(() => {
  if (wss) {
    wss.close();
  }
});
</script>


<style lang="scss" scoped>
.list-item {
    display: block;
    transition: all 1s ease;
    position: relative;
  }
  
  /* 過渡效果 */
  .list-move, .list-enter-active, .list-leave-active {
    transition: all 1s ease;
  }
  
  .list-enter-from, .list-leave-to {
    opacity: 0;
    transform: translateY(-10px);
  }

  .font-weight-bold {
    font-weight: 700;
  }
  .main {
      width: 100%;
      background-color: #f1f1f1;
      color: #252526;
      padding-bottom: 1rem;

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
          border-radius: 20px;
          width: 95%;

          .list-item {
              display: flex;
              justify-content: space-around;
              align-items: center;
              padding-top: 1rem;
              border-bottom: 1px solid #f1f1f1;

              .flex1_center {
                flex: 1;
                text-align: center;
              }
              .flex2_center {
                flex: 2;
                text-align: center;
              }              

              .number {
                  position: relative;
                  background-color: #fbe7e9;
                  width: 1rem;
                  height: calc(1rem * cos(30deg) * 2);
                  display: flex;
                  align-items: center;
                  justify-content: center;
              }
              .number::after {
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
              .number::before {
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
                      width: 3rem;
                      height: 3rem;
                      border-radius: 50%;
                  }
              }

              .info {
                  .name {
                      font-size: 1.2rem;
                      display: -webkit-box;
                      overflow: hidden;
                      -webkit-line-clamp: 1;
                      -webkit-box-orient: vertical;
                  }
                  .point{
                      display: flex;
                      align-items: baseline;
                      justify-content: center;
                      color: #ee4d2d;
                      font-size: 1.2rem;

                      .text{
                          color: #9d9d9d;
                          font-size: 1rem;
                          margin-right: .5rem
                      }
                  } 
              }

              .title {
                  .title_default {
                      background: linear-gradient(to bottom right, #451c01, #b5750e);
                      color: snow;
                      padding: .25rem;
                      font-size: 0.7rem;
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
                      background: linear-gradient(to bottom right, #d3b66f, #8a50d0);
                  }
              }

              .icon img{
                  width: 2rem;
                  height: 2rem;
              }
          }
      }
  }
</style>