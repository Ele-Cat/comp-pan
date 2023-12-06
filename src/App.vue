<template>
  <div class="pan-box">
    <div class="pan-list">
      <div class="title">
        <p>聚合盘搜</p>
        <div class="sk-chase" v-if="panLoading">
          <div class="sk-chase-dot"></div>
          <div class="sk-chase-dot"></div>
          <div class="sk-chase-dot"></div>
          <div class="sk-chase-dot"></div>
          <div class="sk-chase-dot"></div>
          <div class="sk-chase-dot"></div>
        </div>
      </div>
      <div class="list">
        <p v-for="(item, index) in lists" :title="item.label" :key="index" @click="handleClick(index)"
          :class="{ active: activePanIndex == index }">{{ item.label }}</p>
      </div>
    </div>
    <div class="pan-frame">
      <div class="empty" v-if="!activePan">请点击左侧选择盘搜源</div>
      <iframe v-else id="iframe" class="iframe" :src="activePan.url" frameborder="0" @load="onFrameLoad"
        @error="onFrameError"></iframe>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed, nextTick } from 'vue';

const lists = reactive([
  {
    url: "https://alipanx.com",
    label: "阿里云盘搜索"
  },
  {
    url: "https://pan666.net/",
    label: "阿里小站1"
  },
  {
    url: "https://myxiaozhan.net/",
    label: "阿里小站2"
  },
  {
    url: "https://hunhepan.com/#/search?page=1&amp;q=",
    label: "混合盘搜索"
  },
  {
    url: "https://pan.justin3go.com/",
    label: "阿里云盘搜索"
  },
  {
    url: "https://www.zhaokeya.com/",
    label: "学搜搜(酷搜)"
  },
  {
    url: "https://www.upyunso.com",
    label: "UP云搜"
  },
  {
    url: "https://www.codelicence.cn/",
    label: "云盘4K"
  },
  {
    url: "https://dapanso.com/",
    label: "大盘搜"
  },
  {
    url: "https://www.alipansou.com/",
    label: "猫狸盘搜"
  },
  {
    url: "https://www.sopandas.com/",
    label: "熊猫搜盘"
  },
  {
    url: "https://www.xiaobaipan.com/",
    label: "小白盘搜索"
  },
  {
    url: "https://www.xiaoso.net/",
    label: "小不点搜索"
  },
  {
    url: "https://www.sobaidupan.com/",
    label: "SO百度盘"
  },
  {
    url: "https://www.fastsoso.cn/",
    label: "Fastsoso"
  },
  {
    url: "https://www.lingfengyun.com/",
    label: "凌风云搜索"
  },
  {
    url: "https://www.pansearch.me/",
    label: "PanSearch"
  },
  {
    url: "https://www.sousuopu.com/",
    label: "学搜搜"
  },
  {
    url: "https://www.magicalsearch.top",
    label: "奇妙搜索"
  },
  {
    url: "https://www.alypw.com/",
    label: "阿里云盘网"
  },
  {
    url: "https://pansou.cc/",
    label: "盘搜"
  },
  {
    url: "https://pansoso.com/",
    label: "盘搜搜"
  },
  {
    url: "https://www.xuebapan.com/",
    label: "学霸盘"
  },
  {
    url: "https://s.6miu.com/",
    label: "6miu"
  },
  {
    url: "https://feiyu100.cn/",
    label: "飞鱼盘搜"
  },
  {
    url: "https://nmme.xyz/",
    label: "橘子盘搜"
  },
  {
    url: "https://www.xiongdipan.com/",
    label: "兄弟盘"
  },
  {
    url: "https://pan.funletu.com/",
    label: "趣盘搜"
  },
  {
    url: "https://aipanso.com/",
    label: "爱盘搜"
  },
  {
    url: "https://www.iizhi.cn",
    label: "毕方铺"
  },
  {
    url: "https://hunhepan.com/#/resource",
    label: "夸克网盘资源"
  },
  {
    url: "https://ali.gitcafe.ink/",
    label: "小纸条"
  },
])

const activePanIndex = ref(localStorage.getItem('activePanIndex') || 0);
nextTick(() => {
  let activePanDOM = document.getElementsByClassName("active")[0];
  if (activePanDOM) {
    activePanDOM.scrollIntoView({block: "center", inline: "nearest"});
  }
})
const activePan = computed(() => {
  panLoading.value = true;
  return lists[activePanIndex.value];
});
const panLoading = ref(false);

const onFrameLoad = () => {
  panLoading.value = false;
}

const onFrameError = () => {
  window.open(activePan.value.url, "_blank");
}

const handleClick = (idx) => {
  panLoading.value = true;
  activePanIndex.value = idx;
  localStorage.setItem('activePanIndex', idx);
}
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100vh;
  overflow: hidden;

  .pan-box {
    height: 100vh;
    display: flex;

    .pan-list {
      border-right: 1px solid #ddd;
      width: 180px;

      .title {
        position: relative;
        height: 40px;
        line-height: 40px;
        text-align: center;
        font-weight: bold;
        background-color: #dedede;

        .sk-chase {
          position: absolute;
          top: 10px;
          right: 10px;
          width: 24px;
          height: 24px;
          animation: sk-chase 2.5s infinite linear both;
        }

        .sk-chase-dot {
          width: 100%;
          height: 100%;
          position: absolute;
          left: 0;
          top: 0;
          animation: sk-chase-dot 2s infinite ease-in-out both;
        }

        .sk-chase-dot:before {
          content: "";
          display: block;
          width: 25%;
          height: 25%;
          background-color: #FD6585;
          border-radius: 100%;
          animation: sk-chase-dot-before 2s infinite ease-in-out both;
        }

        .sk-chase-dot:nth-child(1) {
          animation-delay: -1.1s;
        }

        .sk-chase-dot:nth-child(2) {
          animation-delay: -1s;
        }

        .sk-chase-dot:nth-child(3) {
          animation-delay: -0.9s;
        }

        .sk-chase-dot:nth-child(4) {
          animation-delay: -0.8s;
        }

        .sk-chase-dot:nth-child(5) {
          animation-delay: -0.7s;
        }

        .sk-chase-dot:nth-child(6) {
          animation-delay: -0.6s;
        }

        .sk-chase-dot:nth-child(1):before {
          animation-delay: -1.1s;
        }

        .sk-chase-dot:nth-child(2):before {
          animation-delay: -1s;
        }

        .sk-chase-dot:nth-child(3):before {
          animation-delay: -0.9s;
        }

        .sk-chase-dot:nth-child(4):before {
          animation-delay: -0.8s;
        }

        .sk-chase-dot:nth-child(5):before {
          animation-delay: -0.7s;
        }

        .sk-chase-dot:nth-child(6):before {
          animation-delay: -0.6s;
        }
      }

      .list {
        background-color: #fafafa;
        padding: 10px 0;
        height: calc(100vh - 60px);
        overflow-y: scroll;

        p {
          margin: 0;
          padding-left: 10px;
          line-height: 2;
          font-size: 16px;
          border-bottom: 1px dashed #dadada;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          cursor: pointer;

          &:hover,
          &.active {
            color: #FD6585;
            border-right: 2px solid #FD6585;
          }
        }
      }
    }

    .pan-frame {
      position: relative;
      flex: 1;
      padding: 0 !important;
      height: 100vh;

      .empty {
        text-align: center;
        line-height: 80vh;
      }

      .iframe {
        width: 100%;
        height: 100vh;
      }

      &::after {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        box-shadow: 0 0 20px inset #ddd;
        z-index: 99;
        pointer-events: none;
      }
    }
  }
}

@media screen and (max-width: 640px) {
  #app {
    .pan-box {
      .pan-list {
        width: 88px;

        .list {
          p {
            font-size: 12px;
            padding-left: 4px;
          }
        }
      }
    }
  }
}

@keyframes sk-chase {
  100% {
    transform: rotate(360deg);
  }
}

@keyframes sk-chase-dot {

  80%,
  100% {
    transform: rotate(360deg);
  }
}

@keyframes sk-chase-dot-before {
  50% {
    transform: scale(0.4);
  }

  100%,
  0% {
    transform: scale(1);
  }
}</style>
