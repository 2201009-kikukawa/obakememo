<template>
      <!--<button><router-link to="/">戻る</router-link></button>-->
  <div class="image-container">
    <div class="text-container">
      <div class="overlay-text" ref="textElement"></div>
    </div>
    <img class="img" :src="imagePath" alt="Image">
  </div>
</template>

<script>
export default {
  data() {
    return {
      imagePath: require('@/assets/img/obake1.png') // 画像のパスをここで指定
    };
  },
  mounted() {
    this.splitText();
  },
  methods: {
    splitText() {
      const textElement = this.$refs.textElement;
      const text = "Javaの課題"; // 表示したいテキストを設定
      const characters = text.split("");
      textElement.innerHTML = characters.map(char => `<span>${char}</span>`).join("");
    }
  }
};
</script>

<style>
.img {
  width: 100px;
  height: 100px;
}

.image-container {
  position: relative;
  display: inline-block;
}

.text-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex; /* テキストを横に並べるために Flex レイアウトを使用 */
}

.overlay-text {
  color: white;
  font-size: 18px;
  text-align: center;
  background-color: rgba(0, 0, 0, 0.7);
  padding: 10px;
  opacity: 0; /* 初めは非表示 */
  transition: opacity 0.3s ease-in-out;
  white-space: nowrap; /* テキストを改行させないようにする */
}
.image-container:hover .overlay-text {
  opacity: 1; /* カーソルを合わせたときに表示 */
}
</style>


