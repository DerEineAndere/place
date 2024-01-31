<script setup lang="ts">
import PocketBase from "pocketbase";

const pb = new PocketBase("http://127.0.0.1:8090");
const colorHEX = ref("000000")
//const canvas = await pb.collection("canvases").getOne("lhez19a24ao0nf4");

const pixels = ref (await pb.collection("pixels").getFullList({
  filter: 'canvas_id = "lhez19a24ao0nf4"',
  sort: '-y,x'
}));
/*const pixels = await pb.collection("pixels").getFullList({
  filter: 'canvas_id = "lhez19a24ao0nf4"',
  sort: "-y,x",
});*/
function goTo() {
    navigateTo("/login");
};
import ColorPicker from 'primevue/colorpicker';

async function pixel_click(id:string){
  console.log(colorHEX.value)
  console.log(id)
  const data = {
    "color": "#" + colorHEX.value,
  };

  const record = await pb.collection('pixels').update(id, data);
};
// Subscribe to changes only in the specified record
pb.collection('pixels').subscribe('*', function (e) {
    console.log(e.action);
    console.log(e.record);
    const pixel = pixels.value.find(p => p.id === e.record.id)!
    pixel.color = e.record.color
}, { /* other options like expand, custom headers, etc. */ });


//const pixels = reactive({});
</script>

<template>
  <button v-on:click=goTo()>Logout</button>
  <ColorPicker v-model="colorHEX" inputId="cp-hex" format="hex" class="mb-3" />

  <center>
    <div id="grid">
      <div
        v-for="(pixel, index) in pixels"
        class="pixels"
        :style="{ 'background-color': pixel.color }"
        @click = "pixel_click(pixel.id)"
      >
      </div>
    </div>
  </center>
</template>

<style>
#grid {
  display: grid;
  grid-template-columns: repeat(52, 1fr);
  grid-template-rows: repeat(52, 1fr);
  width: 45%;
  aspect-ratio: 1/1;
  border: 1px solid slategray;
  cursor: pointer;
  align-self: center;
}

.pixels {
  height: 100%;
  width: 100%;
}

.pixels:hover{
  background-color: slategray !important;
}

</style>