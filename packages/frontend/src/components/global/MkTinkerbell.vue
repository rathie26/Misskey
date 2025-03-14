<template>
  <div class="sparkles-container">
    <!-- 반짝이 효과는 JavaScript에서 관리 -->
  </div>
</template>

<script lang="ts">
export default {
  name: "MkTinkerbell",
  data() {
    return {
      colour: "random", // 색상 설정
      sparkles: 50, // 반짝이 개수
      x: 400,
      ox: 400,
      y: 300,
      oy: 300,
      swide: 800,
      shigh: 600,
      sleft: 0,
      sdown: 0,
      tiny: new Array(),
      star: new Array(),
      starv: new Array(),
      starx: new Array(),
      stary: new Array(),
      tinyx: new Array(),
      tinyy: new Array(),
      tinyv: new Array(),
    };
  },
  mounted() {
    // 페이지가 로드될 때 효과 시작
    this.initializeSparkles();
    window.onresize = this.setWidth;
    window.onscroll = this.setScroll;
    document.onmousemove = this.mouse;
  },
  methods: {
    // div 생성 함수
    createDiv(height: number, width: number) {
      const div = document.createElement("div");
      div.style.position = "absolute";
      div.style.height = `${height}px`;
      div.style.width = `${width}px`;
      div.style.overflow = "hidden";
      return div;
    },

    // 반짝이 효과 초기화
    initializeSparkles() {
      let i, rats, rlef, rdow;
      for (i = 0; i < this.sparkles; i++) {
        rats = this.createDiv(3, 3);
        rats.style.visibility = "hidden";
        rats.style.zIndex = "99999";
        document.body.appendChild(this.tiny[i] = rats);
        this.starv[i] = 0;
        this.tinyv[i] = 0;

        rats = this.createDiv(5, 5);
        rats.style.backgroundColor = "transparent";
        rats.style.visibility = "hidden";
        rats.style.zIndex = "99999";
        rlef = this.createDiv(1, 5);
        rdow = this.createDiv(5, 1);
        rats.appendChild(rlef);
        rats.appendChild(rdow);
        rlef.style.top = "2px";
        rlef.style.left = "0px";
        rdow.style.top = "0px";
        rdow.style.left = "2px";
        document.body.appendChild(this.star[i] = rats);
      }
      this.setWidth();
      this.sparkle();
    },

    // 반짝이 위치 및 색상 업데이트
    sparkle() {
      let c;
      if (Math.abs(this.x - this.ox) > 1 || Math.abs(this.y - this.oy) > 1) {
        this.ox = this.x;
        this.oy = this.y;
        for (c = 0; c < this.sparkles; c++) {
          if (!this.starv[c]) {
            this.star[c].style.left = (this.starx[c] = this.x) + "px";
            this.star[c].style.top = (this.stary[c] = this.y + 1) + "px";
            this.star[c].style.clip = "rect(0px, 5px, 5px, 0px)";
            this.star[c].childNodes[0].style.backgroundColor =
              this.star[c].childNodes[1].style.backgroundColor =
                this.colour === "random" ? this.newColour() : this.colour;
            this.star[c].style.visibility = "visible";
            this.starv[c] = 50;
            break;
          }
        }
      }
      for (c = 0; c < this.sparkles; c++) {
        if (this.starv[c]) this.updateStar(c);
        if (this.tinyv[c]) this.updateTiny(c);
      }
      setTimeout(this.sparkle, 40);
    },

    // 별 업데이트
    updateStar(i) {
      if (--this.starv[i] === 25) this.star[i].style.clip = "rect(1px, 4px, 4px, 1px)";
      if (this.starv[i]) {
        this.stary[i] += 1 + Math.random() * 3;
        this.starx[i] += (i % 5 - 2) / 5;
        if (this.stary[i] < this.shigh + this.sdown) {
          this.star[i].style.top = this.stary[i] + "px";
          this.star[i].style.left = this.starx[i] + "px";
        } else {
          this.star[i].style.visibility = "hidden";
          this.starv[i] = 0;
          return;
        }
      } else {
        this.tinyv[i] = 50;
        this.tiny[i].style.top = (this.tinyy[i] = this.stary[i]) + "px";
        this.tiny[i].style.left = (this.tinyx[i] = this.starx[i]) + "px";
        this.tiny[i].style.width = "2px";
        this.tiny[i].style.height = "2px";
        this.tiny[i].style.backgroundColor = this.star[i].childNodes[0].style.backgroundColor;
        this.star[i].style.visibility = "hidden";
        this.tiny[i].style.visibility = "visible";
      }
    },

    // 작은 반짝이 업데이트
    updateTiny(i) {
      if (--this.tinyv[i] === 25) {
        this.tiny[i].style.width = "1px";
        this.tiny[i].style.height = "1px";
      }
      if (this.tinyv[i]) {
        this.tinyy[i] += 1 + Math.random() * 3;
        this.tinyx[i] += (i % 5 - 2) / 5;
        if (this.tinyy[i] < this.shigh + this.sdown) {
          this.tiny[i].style.top = this.tinyy[i] + "px";
          this.tiny[i].style.left = this.tinyx[i] + "px";
        } else {
          this.tiny[i].style.visibility = "hidden";
          this.tinyv[i] = 0;
          return;
        }
      } else this.tiny[i].style.visibility = "hidden";
    },

    newColour() {
      const baseColor = [108, 166, 156]; // 6CA69C (RGB 값)
      const variation = 30; // 주변 색을 생성할 범위 (±30 정도로 변형)

      const r = baseColor[0] + Math.floor(Math.random() * variation * 2) - variation;
      const g = baseColor[1] + Math.floor(Math.random() * variation * 2) - variation;
      const b = baseColor[2] + Math.floor(Math.random() * variation * 2) - variation;

      // RGB 값이 0-255 사이로 보정
      const clampedR = Math.min(Math.max(r, 0), 255);
      const clampedG = Math.min(Math.max(g, 0), 255);
      const clampedB = Math.min(Math.max(b, 0), 255);

      return `rgb(${clampedR}, ${clampedG}, ${clampedB})`;
    },

    // 마우스 좌표 설정
    mouse(e) {
      if (e) {
        this.y = e.pageY;
        this.x = e.pageX;
      } else {
        this.setScroll();
        this.y = event.y + this.sdown;
        this.x = event.x + this.sleft;
      }
    },

    // 스크롤 설정
    setScroll() {
      if (typeof self.pageYOffset === "number") {
        this.sdown = self.pageYOffset;
        this.sleft = self.pageXOffset;
      } else if (document.body && (document.body.scrollTop || document.body.scrollLeft)) {
        this.sdown = document.body.scrollTop;
        this.sleft = document.body.scrollLeft;
      } else if (document.documentElement && (document.documentElement.scrollTop || document.documentElement.scrollLeft)) {
        this.sleft = document.documentElement.scrollLeft;
        this.sdown = document.documentElement.scrollTop;
      } else {
        this.sdown = 0;
        this.sleft = 0;
      }
    },

    // 화면 크기 설정
    setWidth() {
      let sw_min = 999999;
      let sh_min = 999999;
      if (document.documentElement && document.documentElement.clientWidth) {
        if (document.documentElement.clientWidth > 0) sw_min = document.documentElement.clientWidth;
        if (document.documentElement.clientHeight > 0) sh_min = document.documentElement.clientHeight;
      }
      if (typeof self.innerWidth === "number" && self.innerWidth) {
        if (self.innerWidth > 0 && self.innerWidth < sw_min) sw_min = self.innerWidth;
        if (self.innerHeight > 0 && self.innerHeight < sh_min) sh_min = self.innerHeight;
      }
      if (document.body.clientWidth) {
        if (document.body.clientWidth > 0 && document.body.clientWidth < sw_min) sw_min = document.body.clientWidth;
        if (document.body.clientHeight > 0 && document.body.clientHeight < sh_min) sh_min = document.body.clientHeight;
      }
      if (sw_min === 999999 || sh_min === 999999) {
        sw_min = 800;
        sh_min = 600;
      }
      this.swide = sw_min;
      this.shigh = sh_min;
    },
  },
};
</script>

<style scoped>
/* 스타일을 적용하는 부분 */
.sparkles-container {
  position: relative;
  height: 100%;
  width: 100%;
  overflow: hidden;
}
</style>
