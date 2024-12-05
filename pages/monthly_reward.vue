<template>
    <NuxtLayout name="empty">
      <div v-if="data" class="container">
        <!-- <h1>${ data.monthlyNum } 月份活動獎勵查詢</h1> -->
        <h1>獎勵活動查詢</h1>
  
        <!-- [ 特別慶祝活動區 ] -->
        <ActiveZoom />

        <!-- 按鈕切換區 -->
        <div class="level_buttom_zoom">
          <button
            v-for="(item, index) in data.rewardData"
            :key="index"
            :class="{ active: activeIndex === index }"
            @click="setActive(index)"
            type="button"
            class="btn btn-light"
          >
            ${ item.level } 
          </button>
        </div>
  
        <!-- 動態顯示內容區 -->
        <div v-if="currentReward">
          <p>排位活動<b>本月</b>獎勵：</p>
          <!-- <p class="fw-bolder">排位 ${ data.monthlyNum }月活動獎勵（一）說明:</p> -->
          <p>${ data.monthlyNum }${ currentReward.title1 }</p>
          <ul>
            <li v-for="(text, idx) in currentReward.title1_text" :key="idx">
              ${ text }
            </li>
          </ul>
          <br />
          <p>排位活動<b>升等</b>獎勵：</p>
          <!-- <p class="fw-bolder">排位 ${ data.monthlyNum }月活動獎勵（二）說明:</p> -->
          <p>${ currentReward.title2 }</p>
          <ul>
            <li v-for="(text, idx) in currentReward.title2_text" :key="idx">
              ${ text }
            </li>
          </ul>
          <br />
          <p class="fw-bolder">注意事項:</p>
          <ol>
            <li>主辦單位保留隨時修正、暫停、終止或解釋本活動之最終權利 (包括但不限於更換活動、提前終止或延長活動時間之最終決定權等事項)，並以彈珠菓子 活動公告為準。</li>
            <li>活動獎項都為限時限量，例如彈珠能量抽獎轉盤，其獎項如果獎項被抽完，就不在遞補。</li>
            <li>當月活動獎勵如果兌換完，就不在補充，請把握機會盡快換取!</li>
          </ol>          
        </div>
        <div class="reward_picture">
            <img v-for="(picture, index) in data.rewardPictureData" :key="index" :src="`${ picture }`" class="gallery-image" @click="openModal(picture)" alt="Reward picture" />
        </div>

        <!-- Modal 彈窗 -->
        <div
            class="modal fade"
            id="imageModal"
            tabindex="-1"
            aria-labelledby="imageModalLabel"
            aria-hidden="true"
        >
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header d-none">
            </div>
            <div class="modal-body">
              <img :src="selectedImage" class="img-fluid" alt="Selected Reward Image" />
            </div>
          </div>
        </div>
      </div>

      </div>
    </NuxtLayout>
  </template>

<script setup lang="ts">

// 定義狀態
const data = ref<{ rewardData: any[]; monthlyNum: number; rewardPictureData: any[]; } | null>(null);
const currentReward = ref<any>(null);
const activeIndex = ref(0);
const isLoading = ref(false);
const error = ref(null);

// 動態載入 JSON 文件
const loadData = async () => {
  isLoading.value = true;
  error.value = null;

  try {
    const importedData = await import('../assets/data/rewardList.json');
    data.value = importedData.default ?? importedData;

    // 初始化 currentReward
    currentReward.value = data.value.rewardData[0] || null;
  } catch (err) {
    console.error('Error loading data:', err);
    error.value = 'Failed to load data.';
  } finally {
    isLoading.value = false;
  }
};

// 切換活動的邏輯
const setActive = (index: number): void => {
  activeIndex.value = index;
  if (data.value && data.value.rewardData) {
    currentReward.value = data.value.rewardData[index] || null;
  }
};

// 用於存儲當前選中的圖片
const selectedImage = ref<string>('');

// Nuxt 提供的 $bootstrap
const { $bootstrap } = useNuxtApp();

// 打開 Modal，並顯示選中的圖片
const openModal = (imageSrc: string): void => {
  selectedImage.value = imageSrc;
  const modalElement = document.getElementById('imageModal')!;
  $bootstrap.modal(modalElement).show();
};

// 在頁面加載時調用
onMounted(loadData);
</script>


<style lang="scss">
    .container {
        font-size: 1.1rem;
        ul {
            list-style-type: disc;
            padding-left: 2rem; 
        }
        ol {
            list-style-type: decimal;
            padding-left: 2rem; 
        }
        h1 {
            text-align: center;
        }

        .level_buttom_zoom {
            display: flex;
            align-items: center;
            justify-content: space-evenly;
            margin-bottom: 1rem;

            button {
                background-color: rgba(0,0,0,.05);
                font-size: 1.5rem;
            }

            .active {
                border: 3px solid goldenrod;
            }
        }

        .reward_picture {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 2rem;

            img {
                width: 18%;
                object-fit: cover;
                border-radius: 8px;
            }
        }
    }
    .gallery-image {
        cursor: pointer;
        transition: transform 0.3s ease;
    }
    
    @media (max-width: 768px) {
        .container{
            .reward_picture {
                img{
                    width: 100%; /* 手機端顯示 100% 寬度 */
                }    
            }
        }    
    }
</style>