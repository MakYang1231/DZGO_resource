<template>
  <NuxtLayout name="custom-layout">
      <div class="main" v-if="currentDataSet === 1">
          <!-- <div class="topImage">
              <img src="/images/main/1037089_0.jpg" alt="">
          </div> -->
          <!-- <div class="storeItem d-none">
              <div class="item" :class="{ active: selectedStore === 'shilin' }" @click="fetchData('shilin')">士林店</div>
              <div class="item" :class="{ active: selectedStore === 'sanhe' }" @click="fetchData('sanhe')">三和店</div>
          </div> -->
          <ul class="list list-group font-weight-bold list-header">
            <li class="list-item list-group-item">
              <div :class="{'flex2_center': item === '彈珠玩家/戰力值', 'flex1_center': item !== '彈珠玩家/戰力值'}" v-for="(item, index) in title" :key="index"> ${ item } </div>
            </li>
          </ul>
          <div v-for="(group, groupIndex) in sortedItems" :key="groupIndex" class="main-Carousel" v-show="groupIndex === currentIndex">
          <transition-group name="list" tag="ul" mode="out-in" class="list list-group">  
            <li class="list-item list-group-item" v-for="(item, index) in group" :ref="item.user_point_change_indicator === true ? storeRef : null" :class="`list-item-animation list-item-${item.user_rank}`" :key="item.user_id" :style="{ order: item.user_rank, animationDelay: `${index * 0.2}s` }">
                  <div class="d-flex justify-content-center flex1_center">
                    <div class="number">${ item.user_rank }</div>
                  </div>
                  <div class="picture flex1_center">
                    <NuxtImg class="lazyload" itemprop="image" :src="`${ item.user_picture }`" :data-src="`${ item.user_picture }`" /> 
                </div>
                <div class="info flex2_center">
                  <div class="name">${ item.user_name }</div>
                  <div class="point">
                      <div class="text">戰力值</div>
                      <countTo :startVal="0" :endVal="item.user_fight_point" :duration="2000" separator="," class="me-2"/>
                      <img v-if="item.user_rank_up_indicator === true" class="lazyload" itemprop="image" src="/images/ranking/up.png" data-src="images/ranking/up.png" />
                      <!-- ${ formatNumber(item.user_fight_point) } -->
                  </div>
                </div>
                <div class="title flex1_center">
                      <div v-if="item.user_wealth_title === '薄有積蓄'" class="title_gray_50">薄有積蓄</div>
                      <div v-else-if="item.user_wealth_title === '小有成就'" class="title_gray_75">小有成就</div>
                      <div v-else-if="item.user_wealth_title === '生財有道'" class="title_dark">生財有道</div>
                      <div v-else class="title_default title_gray_25">白手起家</div>
                </div>
                <!-- <div class="icon flex1_center">
                  <img v-if="item.user_rank_up_indicator === true" class="lazyload" itemprop="image" src="/images/ranking/up.png" data-src="images/ranking/up.png" /> 
                </div> -->
                <div class="icon flex1_center">
                  <img v-if="item.user_medal === 2" class="lazyload" itemprop="image" src="/images/ranking/medal_silver.png" data-src="/images/ranking/medal_silver.png" /> 
                  <img v-else-if="item.user_medal === 3" class="lazyload" itemprop="image" src="/images/ranking/medal_gold.png" data-src="images/ranking/medal_gold.png" /> 
                  <img v-else-if="item.user_medal === 4" class="lazyload" itemprop="image" src="/images/ranking/medal_diamond.png" data-src="images/ranking/medal_diamond.png" /> 
                  <img v-else class="lazyload" itemprop="image" src="/images/ranking/medal_copper.png" data-src="images/ranking/medal_copper.png"/> 
                </div>
            </li>
          </transition-group>
          </div>
      </div>

      <div class="main" v-if="currentDataSet === 2">
          <ul class="list list-group font-weight-bold list-header">
            <li class="list-item list-group-item">
              <div :class="{'flex2_center': item === '彈珠玩家/戰力值', 'flex1_center': item !== '彈珠玩家/戰力值'}" v-for="(item, index) in title_2" :key="index"> ${ item } </div>
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
                <div class="info flex1_center">
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
  <div v-if="showModal" class="enter-model">
      <div v-if="showModelItem">        
          <img class="model_img" src="/images/main/1037088_0.jpg" alt="菓子大圖">
          <button class="btn btn-light" @click="handleUserInteraction">
              <h1>播放音效</h1>
          </button>
      </div>    
  </div>
  <!-- 影片標籤 -->
  <video id="fullscreenVideo" class="fullscreen-video d-none" controls ref="DOM_video">
        <source src="/video/silver_adv.MOV" type="video/mp4">
        您的瀏覽器不支援影片播放
  </video>      
</template>

<script setup lang="ts">
//import _ from 'lodash';
  const DOM_video = ref();

  definePageMeta({
      layout: false
  });

  const router = useRouter().currentRoute.value;
  const subbranch = router.params.subbranch;
  const DOM_list_item = ref();
  //const DOM_list_item = ref<HTMLElement | null>(null);
  // 仅存储最后一个符合条件的元素
  const storeRef = (el) => {
    if (el) {
      DOM_list_item.value = el;
    }
  };

  const items = ref([]);
  const title = ['今日排名', '頭像', '彈珠玩家/戰力值', '財富值', '累積排位'];

  const items_2 = ref([]); 
  const title_2 = ['全台排名', '頭像', '彈珠玩家', '戰力值', '排位', '分佈'];

  const selectedStore = ref(subbranch);

  const update_on_value = ref(false);

  //輪播 init
  let intervalId: ReturnType<typeof setInterval>;
  const currentIndex = ref(0);
  const currentDataSet = ref(1); // 1 代表資料1，2 代表資料2
  const groupSize = 10; // 設定每幾筆資料為一組
  const default_speed = 10*1000; // 輪播速度 毫秒
  const allList_speed = 4*1000;
  const focus_speed = 5000;

  //-- --//

  let default_title_data = ref();  //上方橫幅 default init
  default_title_data.value = {
      marquee: false,
      text: "即時戰力榜"
  }
  provide('default_title', default_title_data); // 傳遞跑馬燈 provide

  let Marquee_data = ref();  //上方橫幅跑馬燈 init
  Marquee_data.value = {
      marquee: false,
      text: "跑馬燈訊息"
  }    
  provide('marquee', Marquee_data); // 傳遞跑馬燈 provide

  const showModal = ref(true); // 進入頁面顯示狀態
  const showModelItem = ref(true); // 顯示進版圖和按鈕
  const soundAllowed = ref(false); // 用于標記是否允許播放音效
  const handleUserInteraction = () => {
      showModal.value = false;
      showModelItem.value = false;
      soundAllowed.value = true; // 用户交互后允許播放音效

      // DOM_video.value.classList.remove("d-none");
      // DOM_video.value.play();
  };

  // Sorted list based on rank

  // 即時榜整理後的資料
  const sortedItems = computed(() => { 
      const result = [];
      items.value = items.value.slice().sort((a, b) => b.user_fight_point - a.user_fight_point);
      for(let i = 0; i < items.value.length; i += groupSize) {
          result.push(items.value.slice(i, i + groupSize));
      }
      return result;
  });

  const sortedItems_All = computed(() => { 
      const result = [];
      items_2.value = items_2.value.slice().sort((a, b) => b.user_accumulated_fight_point - a.user_accumulated_fight_point);
      for(let i = 0; i < items_2.value.length; i += groupSize) {
          result.push(items_2.value.slice(i, i + groupSize));
      }
      return result;
  });    

async function fetchData(store) {
try {
  const response = await $fetch('https://isnmk.com/point/rank_api');
  //const response = await $fetch('/api/rankings');
  const response_All = await $fetch('https://isnmk.com/point/accumulated_ranking_api');
  //const response_All = await $fetch('/api/all_rankings');
  if (store === 'shilin') {
    updateRankings(response.shilin);
  } else if (store === 'sanhe') {
    updateRankings(response.sanhe);
  }
  else {
      updateRankings([]);
      console.log('No Data');
  }
  updateRankings_All(response_All.all);
} catch (error) {
  console.error('Error fetching data:', error);
}
}

function updateRankings(newData) {
  newData = newData.slice().sort((a, b) => b.user_fight_point - a.user_fight_point);

  if (JSON.stringify(newData) !== JSON.stringify(items.value)) {
      items.value = newData;
      if( update_on_value.value === true ) {
          focusUpdateItem();
      }    
  }
};
function updateRankings_All(newData) {
  if (JSON.stringify(newData) !== JSON.stringify(items.value)) {
      items_2.value = newData;
  }
};

function playLevelVideo() {
  return new Promise((resolve) => {
    DOM_video.value.onended = resolve; // 當影片結束時，resolve Promise
    DOM_video.value.classList.remove("d-none");
    DOM_video.value.play(); // 播放影片
  });
};

async function focusUpdateItem() {
  const userWithRankUp = items.value.find(user => user.user_point_change_indicator === true); // 從API找到更改玩家的資訊
  console.log(`user name : ${userWithRankUp.user_name}, user rank: ${userWithRankUp.user_rank}`);
  let toPage = Math.ceil(userWithRankUp.user_rank / groupSize) - 1; // 計算更改玩家的頁數

  await playLevelVideo();

  currentDataSet.value = 1; // 回到即時戰力榜
  currentIndex.value = toPage; // 定位到更改玩家的頁數  
  update_on_value.value = false; // init 觸發更的值

  // 確保在 Vue 渲染完成後執行
  nextTick(() => {
      console.log('CONSOLE LOG DOM_list_item.value after nextTick:', DOM_list_item.value);
      if (DOM_list_item.value) {
          //await nextTick(); // 添加第二層 nextTick
          DOM_list_item.value.classList.add('list-item-focus');
      }

      // 顯示模態窗口並在一段時間後移除高亮效果
      showModal.value = true;
      setTimeout(() => {
          showModal.value = false;
          if (DOM_list_item.value) {
              DOM_list_item.value.classList.remove('list-item-focus');
          }
          startCarousel(default_speed);
      }, focus_speed);
  });
}

const playSound = () => {
  const audio = new Audio('../sound/ding.mp3');
  audio.play().catch(error => {
      console.error('播放音效失败:', error);
  });
};

function startCarousel(speed: number) {
  intervalId = setInterval(() => {
      // 確定當前資料集
      const currentData = currentDataSet.value === 1 ? sortedItems : sortedItems_All;
      
      // 切換到下一組
      currentIndex.value = (currentIndex.value + 1) % currentData.value.length;
      // 如果當前資料集輪播完畢且沒有在上面進行過資料集切換，則切換到另一個資料集
      if (currentIndex.value === 0 || currentData.value.length === 0) {
          clearInterval(intervalId); // 結束輪播
          currentIndex.value = 0;
          currentDataSet.value = currentDataSet.value === 1 ? 2 : 1;

          if (currentDataSet.value === 1) {
              default_title_data.value = { marquee: false,text: "即時戰力榜" };
              startCarousel(default_speed); // 開始輪播
          }
          else if(currentDataSet.value === 2) {
              default_title_data.value = { marquee: false,text: "全台戰力榜" };
              startCarousel(allList_speed); // 開始輪播
          }    
      }
  }, speed);
}

let wss;  

onMounted(() => {

  // 當影片播放結束時，隱藏影片
  DOM_video.value.addEventListener('ended', () => {
    DOM_video.value.classList.add('d-none');
  });

fetchData(subbranch); // 初次加载时获取数据

startCarousel(default_speed); // 開始輪播

  // Connect to WebSocket server
  wss = new WebSocket('wss://isnmk.com/websocket/ranking/');
  console.log(wss);

  wss.onopen = function(event) {
    console.log('WebSocket connection successfully opened');
    // 你可以在这里执行连接成功后的操作
  };

  wss.onmessage = (event) => {
    console.log('WebSocket message received:', event.data);
    update_on_value.value = true;
    // Trigger API refresh when WebSocket message is received
    if (soundAllowed.value) {
      playSound();
    }
    //Marquee_data.value = { marquee: true, text: "恭喜 玩家 Mak 獲得戰力點 1,000 ! "}
    default_title_data.value = { marquee: false,text: "即時戰力榜" };
    fetchData(selectedStore.value);
    clearInterval(intervalId); // 結束輪播
  };

  wss.onerror = (error) => {
    console.error('WebSocket Error:', error);
  };
});

onUnmounted(() => {
  clearInterval(intervalId); // 結束輪播
  if (wss) {
    wss.close();
  }
});
</script>


<style lang="scss" scoped>

.fullscreen-video {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 9999;
}

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
                font-size: 2rem;
              }
              .flex2_center {
                flex: 2;
                text-align: center;
                font-size: 2rem;
              }              

              .number {
                  position: relative;
                  //background-color: #fbe7e9;
                  width: 1rem;
                  height: calc(1rem * cos(30deg) * 2);
                  display: flex;
                  align-items: center;
                  justify-content: center;
                  font-size: 2rem;
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
                      width: 8.5rem;
                      height: 8.5rem;
                      border-radius: 50%;
                  }
              }

              .info {
                  .name {
                      font-size: 2rem;
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
                    font-size: 2.4rem;
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
                  width: 8.5rem;
                  height: 8.5rem;
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