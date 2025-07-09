<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
gsap.registerPlugin(ScrollTrigger)

const slides = [
  {
    img: '/images/R-C.jpg',
    slogan: '晋祠 | 千年古韵 | 华夏文明发源地',
  },
  {
    img: '/images/12958949_153759336314_2.jpg',
    slogan: '壶口瀑布 | 黄河之魂 | 壮美山河',
  },
  {
    img: 'https://www.sxwlw.com/public/upload/business/2017/12-26/74a7dd7fdb3f180687b73de866114858.jpg',
    slogan: '云冈石窟 | 世界文化遗产 | 佛教艺术瑰宝',
  },
]
const currentSlide = ref(0)
let timer = null
const loading = ref(true)

function initGsapAnimations() {
  // 分区弹性动画
  gsap.utils.toArray('.section').forEach((section, i) => {
    gsap.from(section, {
      opacity: 0,
      y: 80,
      duration: 1.1,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: section,
        start: 'top 80%',
        toggleActions: 'play none none none',
      }
    })
  })
  // 卡片弹性动画
  gsap.utils.toArray('.card').forEach((card, i) => {
    gsap.from(card, {
      opacity: 0,
      y: 60,
      scale: 0.92,
      duration: 0.7,
      delay: i * 0.08,
      ease: 'back.out(1.7)',
      scrollTrigger: {
        trigger: card,
        start: 'top 90%',
        toggleActions: 'play none none none',
      }
    })
  })
}

onMounted(async () => {
  timer = setInterval(() => {
    currentSlide.value = (currentSlide.value + 1) % slides.length
  }, 3500)
  setTimeout(async () => {
    loading.value = false
    await nextTick()
    initGsapAnimations()
  }, 1500)
})
onUnmounted(() => {
  clearInterval(timer)
})

const spots = [
  {
    img: 'https://img.pconline.com.cn/images/upload/upc/tx/photoblog/1812/14/c8/123803753_1544800419463.jpg',
    title: '平遥古城',
    desc: '世界文化遗产，保存最完整的古代县城之一。',
  },
  {
    img: '/images/12958949_153759336314_2.jpg',
    title: '壶口瀑布',
    desc: '黄河上最壮观的瀑布，气势磅礴。',
  },
  {
    img: 'https://www.sxwlw.com/public/upload/business/2017/12-26/74a7dd7fdb3f180687b73de866114858.jpg',
    title: '云冈石窟',
    title: '云冈石窟',
    desc: '中国四大石窟之一，佛教艺术瑰宝。',
  },
  {
    img: '/images/R-C.jpg',
    title: '晋祠',
    desc: '中国现存最早的皇家园林之一，集古建、雕塑、碑刻于一体，历史悠久。',
  },
]
const foods = [
  {
    img: '/images/4173-khstaxr9346052.jpg',
    title: '刀削面',
    desc: '山西最具代表性的面食，筋道爽滑。',
  },
  {
    img: '/images/part-00504-2997.jpg',
    title: '过油肉',
    desc: '色泽金黄，外酥里嫩，山西家常菜代表。',
  },
  {
    img: 'https://image.135editor.com/files/users/640/6409669/201907/ZbnqyabE_fqYd.jpg',
    title: '莜面栲栳栳',
    desc: '独特的莜面美食，造型精巧，口感丰富。',
  },
]
const heritage = [
  {
    img: 'https://ts1.tc.mm.bing.net/th/id/R-C.3c74a212b592394b3f6e36946b12785f?rik=duMYrc8EamvOVQ&riu=http%3a%2f%2fn.sinaimg.cn%2fsinakd20230201s%2f617%2fw1267h950%2f20230201%2f3934-db9adb520f7ab11fec492a111fec230c.png&ehk=IDgIilzukT0b56HsgrIuEyJ35qThVmi2jDwVEtot5K8%3d&risl=&pid=ImgRaw&r=0',
    title: '山西剪纸',
    desc: '色彩鲜明，造型生动，传承千年。',
  },
  {
    img: 'https://n.sinaimg.cn/sinacn10118/211/w2048h1363/20181208/90ee-hpfycet7263357.jpg',
    title: '晋剧',
    desc: '山西地方戏曲，唱腔高亢激越。',
  },
]
const figures = [
  {
    img: '/images/guanyu.jpg',
    title: '关羽',
    desc: '三国名将，山西运城人，忠义仁勇，被尊为"武圣"。',
  },
  {
    img: '/images/yanxishan.jpg',
    title: '阎锡山',
    desc: '近代著名政治家、军事家，山西五台人。',
  },
  {
    img: '/images/wangzhihuan.jpg',
    title: '王之涣',
    desc: '盛唐著名诗人，山西人，代表作《登鹳雀楼》。',
  },
]

const popup = ref({ show: false, img: '', title: '', desc: '' })
function showPopup(item) {
  popup.value = { show: true, img: item.img, title: item.title, desc: item.desc }
}
function closePopup() {
  popup.value.show = false
}

// 平滑滚动
if (typeof window !== 'undefined') {
  window.addEventListener('DOMContentLoaded', () => {
    document.querySelectorAll('.guofeng-navbar a').forEach(a => {
      a.addEventListener('click', function(e) {
        const href = this.getAttribute('href')
        if (href && href.startsWith('#')) {
          e.preventDefault()
          const el = document.querySelector(href)
          if (el) el.scrollIntoView({ behavior: 'smooth' })
        }
      })
    })
  })
}

function addInkRipple(e) {
  const btn = e.currentTarget
  const ink = document.createElement('span')
  ink.className = 'button-ink'
  const rect = btn.getBoundingClientRect()
  const size = Math.max(rect.width, rect.height)
  ink.style.width = ink.style.height = size + 'px'
  ink.style.left = (e.clientX - rect.left - size/2) + 'px'
  ink.style.top = (e.clientY - rect.top - size/2) + 'px'
  btn.appendChild(ink)
  ink.addEventListener('animationend', () => ink.remove())
}

// 分享功能
function sharePage(e) {
  if (e) addInkRipple(e)
  if (navigator.share) {
    navigator.share({
      title: '吾乡宣传页',
      text: '快来看看山西的风土人情吧！',
      url: window.location.href
    })
  } else {
    copyToClipboard(window.location.href)
    alert('链接已复制，可手动分享给好友！')
  }
}
function copyToClipboard(text) {
  const input = document.createElement('input')
  input.value = text
  document.body.appendChild(input)
  input.select()
  document.execCommand('copy')
  document.body.removeChild(input)
}
</script>

<template>
  <div class="scroll-snap-container">
    <!-- 欢迎页 -->
    <section class="section-snap welcome-section">
      <svg class="welcome-cloud" width="180" height="60" viewBox="0 0 180 60" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M20 40 Q60 10 90 30 T160 40" stroke="#c7a96b" stroke-width="5" fill="none" opacity="0.7">
          <animate attributeName="stroke-dasharray" values="0,300;120,180;0,300" dur="2s" repeatCount="indefinite"/>
        </path>
      </svg>
      <div class="welcome-title">山西欢迎你</div>
      <div class="welcome-tip">我为家乡代言</div>
      <div class="welcome-intro">
        山西，位于中国华北，素有"表里山河"之称，是中华文明的重要发祥地之一。这里地形以山地、丘陵为主，黄河、汾河穿境而过，孕育了灿烂的农耕文明。山西历史悠久，拥有五千多年文明史，是华夏民族的重要发源地。这里不仅有世界文化遗产云冈石窟、平遥古城、五台山等名胜古迹，更有丰富的非遗文化和独特的民俗风情。山西以其深厚的历史底蕴、丰富的自然资源和独特的地域文化，成为中华大地上一颗璀璨的明珠。
      </div>
      <div class="welcome-poem">"欲穷千里目，更上一层楼。"</div>
    </section>
    <!-- 名胜古迹 -->
    <section class="section-snap" id="spots">
      <div class="section-title">名胜古迹</div>
      <div class="section-divider"></div>
      <div class="heritage-list">
        <div class="card" v-for="(item, idx) in spots" :key="idx" @click="showPopup(item)" style="cursor:pointer;">
          <img :src="item.img" :alt="item.title">
          <div class="card-img-mask"></div>
          <div><strong>{{ item.title }}</strong></div>
          <div>{{ item.desc }}</div>
        </div>
      </div>
    </section>
    <!-- 美食推荐 -->
    <section class="section-snap" id="foods">
      <div class="section-title">美食推荐</div>
      <div class="section-divider"></div>
      <div class="food-list">
        <div class="card" v-for="(item, idx) in foods" :key="idx" @click="showPopup(item)" style="cursor:pointer;">
          <img :src="item.img" :alt="item.title">
          <div class="card-img-mask"></div>
          <div><strong>{{ item.title }}</strong></div>
          <div>{{ item.desc }}</div>
        </div>
      </div>
    </section>
    <!-- 山西人物 -->
    <section class="section-snap" id="figures">
      <div class="section-title">山西人物</div>
      <div class="section-divider"></div>
      <div class="figures-row">
        <div class="card figures-card" v-for="(item, idx) in figures" :key="idx" @click="showPopup(item)" style="cursor:pointer; text-align: center;">
          <div class="figures-img-box">
            <img :src="item.img" :alt="item.title">
          </div>
          <div class="card-img-mask"></div>
          <div><strong>{{ item.title }}</strong></div>
          <div>{{ item.desc }}</div>
        </div>
      </div>
    </section>
    <!-- 非遗文化 -->
    <section class="section-snap" id="heritage">
      <div class="section-title">非遗文化</div>
      <div class="section-divider"></div>
      <div class="heritage-list">
        <div class="card" v-for="(item, idx) in heritage" :key="idx" @click="showPopup(item)" style="cursor:pointer;">
          <img :src="item.img" :alt="item.title">
          <div class="card-img-mask"></div>
          <div><strong>{{ item.title }}</strong></div>
          <div>{{ item.desc }}</div>
        </div>
      </div>
    </section>
    <!-- 结尾页 -->
    <section class="section-snap end-section">
      <div class="end-title">感谢您的浏览</div>
      <svg class="end-cloud" width="120" height="48" viewBox="0 0 120 48" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M10 38 Q30 10 60 24 T110 38" stroke="#c7a96b" stroke-width="4" fill="none" opacity="0.7"/>
      </svg>
      <div class="end-tip">扫码了解更多山西美景美食</div>
      <img src="/images/weixin.jpg" alt="山西文旅二维码" class="qr-img">
      <button class="guofeng-share-fab" @click="sharePage">
        <svg width="36" height="36" viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g>
            <ellipse cx="18" cy="18" rx="16" ry="16" fill="#fffbe6" stroke="#c7a96b" stroke-width="2"/>
            <path d="M10 24 Q18 10 26 24" stroke="#c7a96b" stroke-width="2.2" fill="none"/>
            <path d="M18 10 Q19 18 26 18" stroke="#c7a96b" stroke-width="1.2" fill="none"/>
            <path d="M18 10 Q17 18 10 18" stroke="#c7a96b" stroke-width="1.2" fill="none"/>
            <ellipse cx="18" cy="26" rx="3.5" ry="1.2" fill="#c7a96b" fill-opacity="0.18"/>
          </g>
        </svg>
        <span class="share-text">分享</span>
      </button>
    </section>
    <!-- 弹窗/大图预览 -->
    <div v-if="popup.show" class="popup-mask" @click.self="closePopup">
      <div class="popup-content">
        <img :src="popup.img" :alt="popup.title" class="popup-img">
        <div class="popup-title">{{ popup.title }}</div>
        <div class="popup-desc">{{ popup.desc }}</div>
        <button class="popup-close" @click="closePopup">关闭</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
html, body, #app {
  height: 100%;
  min-height: 100%;
  margin: 0;
  padding: 0;
}
.scroll-snap-container {
  min-height: 100vh;
  height: auto;
  overflow-y: visible;
  scroll-snap-type: y mandatory;
}
.section-snap {
  min-height: 100vh;
  scroll-snap-align: start;
  /* 保留隐藏滚动条样式 */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE 10+ */
}
.section-snap::-webkit-scrollbar {
  display: none; /* Chrome/Safari/Webkit */
}
.welcome-section {
  background: linear-gradient(120deg, #f8f6f1 80%, #e0c097 100%);
}
.welcome-title {
  font-family: 'STKaiti', 'KaiTi', 'Noto Serif SC', serif;
  font-size: 3.2em;
  color: #a0522d;
  letter-spacing: 0.22em;
  text-shadow: 0 2px 18px #c7a96b44;
  margin-bottom: 18px;
  opacity: 0;
  animation: bannerTitleIn 1.2s 0.5s cubic-bezier(.7,-0.1,.5,1.2) forwards;
}
@keyframes bannerTitleIn {
  from { opacity: 0; transform: translateY(40%); }
  to { opacity: 1; transform: translateY(0); }
}
.welcome-cloud {
  margin-bottom: 32px;
  animation: cloudMove 2.2s linear infinite;
}
@keyframes cloudMove {
  0% { transform: translateY(0); }
  50% { transform: translateY(-8px); }
  100% { transform: translateY(0); }
}
.welcome-tip {
  font-size: 1.2em;
  color: #bfa97a;
  letter-spacing: 0.12em;
  margin-bottom: 12px;
}
.end-section {
  background: linear-gradient(120deg, #f8f6f1 80%, #e0c097 100%);
}
.end-title {
  font-family: 'STKaiti', 'KaiTi', 'Noto Serif SC', serif;
  font-size: 2.2em;
  color: #a0522d;
  letter-spacing: 0.18em;
  margin-bottom: 18px;
  text-shadow: 0 2px 12px #c7a96b33;
}
.end-cloud {
  margin-bottom: 18px;
}
.end-tip {
  font-size: 1.1em;
  color: #bfa97a;
  letter-spacing: 0.12em;
  margin-bottom: 10px;
}
.qr-img {
  margin-top: 8px;
  border-radius: 12px;
  width: 120px;
  height: 120px;
}
.guofeng-cloud {
  display: block;
  margin: 0 auto 0.5em auto;
  max-width: 180px;
}
.guofeng-title h1 {
  font-family: 'Songti SC', 'Noto Serif SC', serif;
  font-size: 2.6em;
  font-weight: bold;
  letter-spacing: 0.18em;
  background: linear-gradient(90deg, var(--guofeng-gold) 10%, #fffbe6 90%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 2px 12px #c7a96b33;
  margin-bottom: 0.3em;
}
.guofeng-title .slogan {
  font-family: 'KaiTi', 'STKaiti', 'Noto Serif SC', serif;
  font-size: 1.3em;
  color: var(--guofeng-primary);
  letter-spacing: 0.12em;
  text-shadow: 0 2px 8px #fffbe6aa;
}
.guofeng-slogan {
  font-family: 'KaiTi', 'STKaiti', 'Noto Serif SC', serif;
  color: var(--guofeng-gold);
  font-size: 1.25em;
  letter-spacing: 0.18em;
  margin-bottom: 1.2em;
  text-shadow: 0 2px 8px #c7a96b33;
}
.intro-img {
  width: 90%;
  max-width: 400px;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  margin-bottom: 18px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.intro-stats {
  display: flex;
  gap: 24px;
  justify-content: center;
  margin-top: 16px;
  flex-wrap: wrap;
}
.stat-num {
  font-size: 2em;
  font-weight: bold;
}
.hometown-msg {
  font-size: 1.3em;
  line-height: 1.8;
  margin: 24px 0;
  max-width: 500px;
  text-align: center;
  margin-left: auto;
  margin-right: auto;
}
.heritage-list, .food-list {
  display: block;
  width: 100%;
  margin: 0 auto;
}
.card {
  width: 95vw;
  max-width: 1200px;
  min-width: 260px;
  height: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  box-sizing: border-box;
  padding: 36px 32px 24px 32px;
  background: #fffbe6;
  border-radius: 22px;
  box-shadow: 0 2px 18px rgba(199,169,107,0.13);
  margin: 0 auto 36px auto;
}
.card img {
  width: 100%;
  aspect-ratio: 16/9;
  height: auto;
  object-fit: cover;
  border-radius: 16px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.10);
  margin-bottom: 18px;
}
.card strong {
  font-size: 1.18em;
  color: #a0522d;
  font-family: 'KaiTi', 'STKaiti', 'Noto Serif SC', serif;
  margin-bottom: 6px;
  display: block;
}
.card div {
  font-size: 1.08em;
  color: #3a2c1a;
  line-height: 1.7;
  letter-spacing: 0.04em;
  text-align: center;
}
.card-img-mask {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  bottom: 18px;
  width: 95%;
  height: 24px;
  background: linear-gradient(to top, rgba(34,58,75,0.07), rgba(0,0,0,0));
  z-index: 2;
  border-radius: 0 0 12px 12px;
  pointer-events: none;
  /* 更轻柔的水墨祥云遮罩 */
  background-image: url('data:image/svg+xml;utf8,<svg width="100%25" height="100%25" viewBox="0 0 200 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M10 18 Q60 2 100 18 T190 18" stroke="%23c7a96b" stroke-width="2" fill="none" opacity="0.12"/></svg>');
  background-repeat: no-repeat;
  background-position: center bottom;
}
.section-title {
  font-family: 'Songti SC', 'Noto Serif SC', serif;
  font-size: 2.1rem;
  font-weight: bold;
  color: var(--guofeng-primary);
  margin-bottom: 12px;
  position: relative;
  padding-left: 18px;
  letter-spacing: 0.08em;
}
.section-divider {
  width: 60px;
  height: 4px;
  background: var(--guofeng-gold);
  border-radius: 2px;
  margin: 0 0 24px 18px;
}
.card {
  font-size: 1.12rem;
}
.section p, .card div {
  font-size: 1.08em;
  color: #3a2c1a;
  line-height: 1.85;
  letter-spacing: 0.04em;
}
.intro-stats, .hometown-msg {
  font-family: 'Noto Serif SC', 'Songti SC', serif;
}
.guofeng-navbar {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(248,245,239,0.96);
  display: flex;
  justify-content: center;
  gap: 32px;
  padding: 12px 0 8px 0;
  border-bottom: 2px solid var(--guofeng-gold);
  box-shadow: 0 2px 12px #c7a96b11;
}
.guofeng-navbar a {
  font-family: 'Songti SC', 'Noto Serif SC', serif;
  font-size: 1.13em;
  color: var(--guofeng-primary);
  text-decoration: none;
  padding: 4px 16px;
  border-radius: 18px;
  transition: background 0.2s, color 0.2s;
}
.guofeng-navbar a:hover, .guofeng-navbar a:focus {
  background: var(--guofeng-gold);
  color: #fff;
}
html {
  scroll-behavior: smooth;
}
.popup-mask {
  position: fixed;
  left: 0; top: 0; right: 0; bottom: 0;
  background: rgba(34,58,75,0.32);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: fadeIn 0.3s;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
.popup-content {
  background: #fffbe6;
  border-radius: 22px;
  box-shadow: 0 8px 40px #c7a96b44;
  padding: 32px 24px 18px 24px;
  max-width: 92vw;
  max-height: 90vh;
  text-align: center;
  position: relative;
  border: 3px solid var(--guofeng-gold);
  font-family: 'Noto Serif SC', 'Songti SC', serif;
}
.popup-img {
  width: 60vw;
  max-width: 480px;
  max-height: 48vh;
  object-fit: cover;
  border-radius: 14px;
  margin-bottom: 18px;
  box-shadow: 0 2px 12px #c7a96b22;
}
.popup-title {
  font-family: 'KaiTi', 'STKaiti', 'Noto Serif SC', serif;
  font-size: 1.5em;
  color: var(--guofeng-red);
  margin-bottom: 10px;
  letter-spacing: 0.08em;
}
.popup-desc {
  font-size: 1.13em;
  color: #3a2c1a;
  margin-bottom: 18px;
  line-height: 1.8;
}
.popup-close {
  background: var(--guofeng-gold);
  color: #fff;
  border: none;
  border-radius: 18px;
  padding: 8px 32px;
  font-size: 1.1em;
  font-family: 'Songti SC', 'Noto Serif SC', serif;
  cursor: pointer;
  margin-top: 8px;
  transition: background 0.2s;
}
.popup-close:hover {
  background: var(--guofeng-primary);
}
@media (max-width: 800px) {
  .popup-content {
    padding: 12px 4vw 8px 4vw;
  }
  .popup-img {
    width: 92vw;
    max-width: 98vw;
    max-height: 32vh;
  }
}
.guofeng-share-fab {
  margin: 24px auto 0 auto;
  background: #fffbe6ee;
  border: 2.5px solid var(--guofeng-gold, #c7a96b);
  border-radius: 50px;
  box-shadow: 0 4px 24px #c7a96b33;
  padding: 8px 18px 8px 12px;
  display: flex;
  align-items: center;
  gap: 6px;
  font-family: 'KaiTi', 'STKaiti', 'Noto Serif SC', serif;
  font-size: 1.18em;
  color: var(--guofeng-primary, #a0522d);
  cursor: pointer;
  transition: box-shadow 0.18s, background 0.18s;
  outline: none;
  user-select: none;
}
.guofeng-share-fab:hover {
  background: #fffbe6;
  box-shadow: 0 8px 32px #c7a96b55;
}
.guofeng-share-fab svg {
  display: block;
}
.share-text {
  margin-left: 2px;
  font-size: 1.08em;
  letter-spacing: 0.08em;
}
.button-ink {
  position: absolute;
  border-radius: 50%;
  background: radial-gradient(rgba(199,169,107,0.18) 60%, transparent 100%);
  pointer-events: none;
  animation: inkRipple 0.6s linear;
  z-index: 1;
}
@keyframes inkRipple {
  from { transform: scale(0); opacity: 0.7; }
  to { transform: scale(2.2); opacity: 0; }
}
.card-list {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: flex-start;
  gap: 28px;
  margin: 18px 0 0 0;
}
.figures-row {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: stretch;
  gap: 36px;
  margin: 32px 0 0 0;
}
.card.figures-card {
  max-width: 378px;
  min-width: 0;
  height: 613px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  box-sizing: border-box;
  padding: 47px 23px 33px 23px;
  background: #fffbe6;
  border-radius: 21px;
  box-shadow: 0 2px 17px rgba(199,169,107,0.13);
}
.figures-img-box {
  width: 236px;
  height: 419px;
  aspect-ratio: 9/16;
  overflow: hidden;
  border-radius: 17px;
  border: 2.36px solid #e0c097;
  background: #f8f6f1;
  box-shadow: 0 2px 12px rgba(0,0,0,0.10);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 23px auto;
}
.figures-img-box img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.card.figures-card strong {
  font-size: 1.35em;
}
.card.figures-card div {
  font-size: 1.13em;
}
.loading-mask {
  position: fixed;
  left: 0; top: 0; right: 0; bottom: 0;
  background: linear-gradient(120deg, #f8f6f1 80%, #e0c097 100%);
  z-index: 99999;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: opacity 0.6s;
  animation: fadeIn 0.5s;
}
.loading-content {
  text-align: center;
  animation: fadeInUp 0.8s;
}
.loading-cloud {
  margin: 0 auto 18px auto;
  display: block;
  animation: cloudMove 1.5s linear infinite;
}
@keyframes cloudMove {
  0% { transform: translateY(0); }
  50% { transform: translateY(-8px); }
  100% { transform: translateY(0); }
}
.loading-title {
  font-family: 'STKaiti', 'KaiTi', 'Noto Serif SC', serif;
  font-size: 2.2em;
  color: #a0522d;
  letter-spacing: 0.18em;
  margin-bottom: 10px;
  text-shadow: 0 2px 12px #c7a96b33;
}
.loading-tip {
  font-size: 1.1em;
  color: #bfa97a;
  letter-spacing: 0.12em;
}
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}
.welcome-intro {
  text-align: left;
  text-indent: 2em;
  max-width: 600px;
  margin: 18px auto 8px auto;
  font-size: 1.18em;
  color: #3a2c1a;
  line-height: 1.85;
}
.welcome-poem {
  font-family: 'KaiTi', 'STKaiti', 'Noto Serif SC', serif;
  color: #c7a96b;
  font-size: 1.3em;
  margin-top: 8px;
  letter-spacing: 0.12em;
}
</style>
