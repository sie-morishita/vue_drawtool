<template>
  <div>
    <h1>DrawTool</h1>
    <div id="canvas-area">
      <canvas id="myCanvas" v-bind:class="{eraser: canvasMode === 'eraser'}" width="640px" height="800px" @mousedown="dragStart" @mouseup="dragEnd" @mouseout="dragEnd" @mousemove="draw"></canvas>
    </div>
    <div id="tool-area">
        <button id="pen-black-button" @click="penBlack">ペン（黒）</button>
        <button id="pen-red-button" @click="penRed">ペン（赤）</button>
        <button id="pen-blue-button" @click="penBlue">ペン（青）</button>
        <button id="eraser-button" @click="eraser" >消しゴム</button>
        <button id="clear-button" @click="clear">クリア</button>
        <button id="download-button" @click="download">ダウンロード</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "DrawTool",
  data() {
    return {
        canvasMode: 'penBlack',
        canvas: null,
        context: null,
        isDrag: false,
    };
  },
  mounted(){
      this.canvas = document.querySelector('#myCanvas')
      this.context = this.canvas.getContext('2d')
      this.context.lineCap = 'round';
      this.context.lineJoin = 'round';
      this.context.lineWidth = 5;
      this.context.strokeStyle = '#000000';
  },
  methods: {
    // 描画
    draw :function(e) {
      var x = e.layerX
      var y = e.layerY

      if(!this.isDrag) {
        return;
      }

      this.context.lineTo(x, y);
      this.context.stroke();
    },
    // 描画開始（mousedown）
    dragStart:function(e) {
      var x = e.layerX
      var y = e.layerY

      this.context.beginPath();
      this.context.lineTo(x, y);
      this.context.stroke();
  
      this.isDrag = true;
    },
    // 描画終了（mouseup, mouseout）
    dragEnd: function() {
      this.context.closePath();
      this.isDrag = false;
    },
    // クリア
    clear: function() {
      this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
    },
    // ペンモード（黒）
    penBlack: function(){
      // カーソル変更
      this.canvasMode = 'penBlack'

      // 描画設定
      this.context.lineCap = 'round';
      this.context.lineJoin = 'round';
      this.context.lineWidth = 5;
      this.context.strokeStyle = '#000000';
    },
    // ペンモード（赤）
    penRed: function(){
      // カーソル変更
      this.canvasMode = 'penRed'

      // 描画設定
      this.context.lineCap = 'round';
      this.context.lineJoin = 'round';
      this.context.lineWidth = 5;
      this.context.strokeStyle = '#FF0000';
    },
    // ペンモード（青）
    penBlue: function(){
      // カーソル変更
      this.canvasMode = 'penBlue'

      // 描画設定
      this.context.lineCap = 'round';
      this.context.lineJoin = 'round';
      this.context.lineWidth = 5;
      this.context.strokeStyle = '#0000FF';
    },
    // 消しゴムモード
    eraser: function() {
      // カーソル変更
      this.canvasMode = 'eraser'

      // 描画設定
      this.context.lineCap = 'square';
      this.context.lineJoin = 'square';
      this.context.lineWidth = 30;
      this.context.strokeStyle = '#FFFFFF';
    },
    // 画像ダウンロード
    download: function() {
        let link = document.createElement("a");
        link.href = this.canvas.toDataURL("image/png");
        link.download = 'canvas-' + new Date().getTime() + '.png';
        link.click();
    }
  }
};
</script>
<style scoped>
#myCanvas {
 border: 1px solid #000000;
}

.eraser {
    cursor: url(../assets/image/eraser.png) 15 15,auto;
}
</style>