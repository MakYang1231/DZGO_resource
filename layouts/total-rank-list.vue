<template>
    <header class="Header_root" ref="DOM_Header_root">
        <div class="Head_content d-flex align-items-center justify-content-center">
            <div class="logo d-none align-items-center justify-content-around" ref="DOM_Header_logo">
                <NuxtLink :to="`/`">
                    <img class="lazyload" itemprop="image" :src="`../images/logo/DZGZ_logo.png`" :data-src="`../images/logo/DZGZ_logo.png`" :title="`彈珠菓子_LOGO`" :alt="`彈珠菓子_LOGO`" />
                </NuxtLink>
                </div>
            <div class="main_item d-flex flex-column align-items-center">
                <div class="item_top top_default" ref="DOM_Header_mitem_top_default">全台戰力榜</div>   
            </div>     
            <div class="toolItem d-flex align-items-center justify-content-around">
                <div class="item search">
                    <BootstrapIcon name="search" />
                </div>
                <div class="item cart">
                    <BootstrapIcon name="cart-plus-fill" />
                </div>
                <div class="item user">
                    <BootstrapIcon name="person-plus-fill" />
                </div>
            </div>
        </div>
    </header>
    <slot />
</template>

<script setup lang="ts">

//const { $bootstrap } = useNuxtApp();
const router = useRouter().currentRoute.value;

const DOM_Header_root = ref();
const DOM_Header_logo = ref();

const DOM_Header_mitem_top_default = ref();

const DOM_MenuModule_paper = ref();
const DOM_MenuModule_Nav = ref();
const DOM_CollBady = ref([]);

</script>

<style lang="scss" scoped>.nav_height {
    height: var(--WEB_custom_nav_height);
}

// main
.Header_root {
    transition: all 0.2s ease-in;
    background-color: var(--WEB_main_color);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    transition: all 0.3s ease-in-out;
    position: relative;
    height: fit-content;
    //background: url(../images/ranking/bg_3.png) 100% / cover no-repeat fixed;

    .Head_content {
        padding: 0 0.5rem;
        transition: all 0.3s ease-in-out;

        .menu_button {
            display: none;
            font-size: 3svh;
        }
        .logo {
            width: 7svh;
            transition: all 0.3s ease-in-out;
        }
        .main_item {
            flex-basis: 100%;
            overflow: hidden;
            white-space: nowrap;
            box-sizing: border-box;
            width: 100%; /* 你可以根據需求調整寬度 */
            text-align: center;
            .item_top {
                //transition: all 0.3s ease-in-out;
                font-family: "Reggae One","ＫＯさきがけ龍爪Ｍ";
                letter-spacing: 0rem;
                font-size: 1rem;
                text-align: center;
                display: inline-block !important;
            }
            .top_marquee {
                animation: marquee 10s linear 1; /* 調整動畫持續時間 */
            }
            .item_bottom {
                .bottom_ul {
                    .bottom_li {
                        .bottom_a {
                            .icon {
                                width: 1.5rem;
                                height: 1.5rem;
                            }
                        }
                        .bottom_a:before {
                            background: #fff;
                            content: "";
                            position: absolute;
                            left: 0;
                            bottom: 0;
                            transition: 0.3s;
                            width: 100%;
                            height: 2px;
                            transform-origin: left top;
                            transform: scale(0, 1);
                        }

                        .subItem {
                            background: snow;
                            position: fixed;
                            top: calc(var(--WEB_custom_nav_height) - 1rem);
                            margin: auto;
                            width: 100%;
                            transition: 0.5s;
                            transform: translateY(1em);
                            opacity: 0;
                            visibility: hidden;
                            z-index: -1;
                            left: auto;
                            width: fit-content;
                            max-width: 280px;
                            box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);

                            ul {
                                li {
                                    padding: 1rem 2rem;
                                }
                            }
                        }
                    }
                    .bottom_li:hover {
                        .bottom_a:before {
                            background: #000;
                            transform: scale(1, 1);
                        }
                        .subItem {
                            opacity: 1;
                            visibility: visible;
                            z-index: 1;
                            top: calc(var(--WEB_custom_nav_height) - 1.5rem);
                        }
                    }
                }
            }
        }
        .toolItem {
            //display: none !important;
            font-size: 1.5rem;
            background-color: rgba(200, 200, 200, 0.3);
            border-radius: 10px;
            display: none !important;

            .item {
                padding: .5rem 1rem;

                i {
                    cursor: pointer;
                }
            }
        }

        
    }
}

// [ Menu 主要大項 ]
.MenuModule {
        inset: 0px;
        z-index: 1200;

        .MenuModule_backdrop {
            inset: 0px;
            background-color: rgba(0, 0, 0, .5);
            -webkit-tap-highlight-color: transparent;
            z-index: -1;
        }
        .MenuModule_paper {
            //transform: translateX(-100%);
            background-color: var(--WEB_main_color);
            color: rgba(0, 0, 0, .87);
            box-shadow: rgba(0, 0, 0, 0.2) 0px 8px 10px -5px, 
            rgba(0, 0, 0, 0.14) 0px 16px 24px 2px, 
            rgba(0, 0, 0, 0.12) 0px 6px 30px 5px;
            overflow-y: auto;
            height: 100%;
            flex: 1 0 auto;
            z-index: 1200;
            outline: 0px;

            nav {
                height: 100svh;
                width: 85svw;
                max-width: 85svw;
                overflow: inherit;

                .MainNav_wrapper {
                    padding: 28px 28px 50px 60px;

                    .MainNav_close {
                        text-align: right;
                        padding-bottom: 0;

                        .icon {
                            padding: 5px;
                            cursor: pointer;
                            font-size: 2rem;
                        }
                    }
                    .MainNav_navNav {
                        padding-right: 32px;

                        .MainNav_menu {
                            margin-bottom: 6px;
                            padding: 0px;

                            li {
                                list-style: none;
                                padding: 0 0 8px 0;
                                text-align: left;

                                .bg_s {
                                    width: 120px;
                                    height: 100%;
                                    max-width: inherit;
                                    -o-object-fit: cover;
                                    object-fit: cover;
                                }
                            }
                            li.MainNav_menu_logo {
                                text-align: center;
                            }
                        }
                        .MainNav_Cat {
                            border-top: 1px solid #eaeaea;
                            margin-top: 0px;
                            padding-top: 6px;

                                .MainNav_Cat_link {
                                    padding-top: 14px;
                                    font-size: 14px;
                                    font-weight: 500;
                                    line-height: 25px;
                                    letter-spacing: .65em;
                                    margin: 0;
                                    color: #000;
                                    text-transform: uppercase;
                                    border: none;
                                    text-decoration: none;

                                    .MainNav_Cat_Head {
                                        display: flex;
                                        align-items: baseline;
                                        justify-content: space-between;
                                        font-size: 18px;
                                        padding: 13px;
                                        transition: .15s cubic-bezier(0, 0, 0.2, 1) 0ms;
                                    }

                                    .MainNav_Cat_item {
                                        .preIcon {
                                                width: 2rem;
                                                margin-right: .5rem;
                                            }
                                    }
                                    .MainNav_Cat_item:hover {
                                        .MainNav_Cat_Head {
                                            color: #d66b2d;
                                            transition: all .2s linear; 
                                        }
                                    }

                                    .MainNav_Cat_Body {
                                        background-color: lightgoldenrodyellow;
                                        text-align: left;

                                        .MainNav_Cat_Sub {
                                            padding: 13px 20px 13px 13px;
                                            cursor: pointer;
                                            transition: .15s cubic-bezier(0, 0, 0.2, 1) 0ms;
                                        }
                                    }
                                }
                        }
                    }
                }
            }
        }
    }    
   
    .Header_root {
        display: block;
        .Head_content {
            .logo{
                width: 7svh;
            }
            .main_item {
                display: block !important;
            }
        }
    }

// [ Media screen]
@media screen and (max-width: 767px) {
    // 變數
    .Header_root {
        height: unset;
        .Head_content {
            .menu_button {
                display: block;
            }
            .main_item {
                .item_top {
                    letter-spacing: .5rem;
                    font-size: 1.1rem;
                }
            }
        }    
    }    
}

// [ Animation init ]
    
@keyframes menuPaper_show {
    from {
        transform: translateX(-100%);
    }

    to {
        transform: translateX(0%);
    }
}
@keyframes menuPaper_hide {
    from {
        transform: translateX(0%);
    }

    to {
        transform: translateX(-100%);
    }
}

@keyframes marquee {
  0% {
    transform: translateX(100%); /* 開始時在右側外部 */
  }
  100% {
    transform: translateX(-100%); /* 結束時在左側外部 */
  }
}
</style>