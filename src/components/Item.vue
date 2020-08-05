<template>
  <div>
    <div class="item grid grid-cols-8">
      <div class="col-span-7">
        <span v-html="hasil.text">{{hasil.text}}</span>
      </div>
      <div class="col-span-1 flex flex-col items-center justify-center">
        <svg
          class="yt"
          version="1.1"
          id="Capa_1"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          x="0px"
          y="0px"
          viewBox="0 0 512 512"
          style="enable-background:new 0 0 512 512;"
          xml:space="preserve"
        >
          <path
            d="M490.24,113.92c-13.888-24.704-28.96-29.248-59.648-30.976C399.936,80.864,322.848,80,256.064,80
			c-66.912,0-144.032,0.864-174.656,2.912c-30.624,1.76-45.728,6.272-59.744,31.008C7.36,138.592,0,181.088,0,255.904
			C0,255.968,0,256,0,256c0,0.064,0,0.096,0,0.096v0.064c0,74.496,7.36,117.312,21.664,141.728
			c14.016,24.704,29.088,29.184,59.712,31.264C112.032,430.944,189.152,432,256.064,432c66.784,0,143.872-1.056,174.56-2.816
			c30.688-2.08,45.76-6.56,59.648-31.264C504.704,373.504,512,330.688,512,256.192c0,0,0-0.096,0-0.16c0,0,0-0.064,0-0.096
			C512,181.088,504.704,138.592,490.24,113.92z M192,352V160l160,96L192,352z"
          />
        </svg>
        <a :href="`${url}&t=${hasil.start}s`" target="_blank">Play</a>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["hasil", "url"],
};
</script>
<style scoped>
.yt {
  fill: #092532;
  width: 20px;
  height: 20px;
}
</style>