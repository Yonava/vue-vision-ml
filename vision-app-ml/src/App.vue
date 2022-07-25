<template>
  <div id="app">
    <div>
			<h1>
				Image Feedback
			</h1>
			<div class="camera">
				<video autoplay id="video">Video stream not available.</video>
			</div>
			<canvas id="canvas"></canvas>
			<div class="output">
				<img id="photo" alt="Initalizing">
			</div>
		</div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      width: 800,
      height: 600,
      streaming: false,
      video: null,
      canvas: null,
      photo: null
    }
  },
  mounted() {
    this.startUp();
    setInterval(this.takePicture, 1000);
  },
  methods: {
    takePicture() {

      const canvas = document.querySelector('canvas')
      const photo = document.getElementById('photo')

      var context = canvas.getContext('2d');
			canvas.width = this.width;
			canvas.height = this.height;
			context.drawImage(document.querySelector('video'), 0, 0, this.width, this.height);

			let data = canvas.toDataURL('image/png');
			photo.setAttribute('src', data);
    },
    startUp() {
      
      const video = document.querySelector('video');

      navigator.mediaDevices.getUserMedia({ video: true })
        .then(function(stream) {
          video.srcObject = stream;
        })
        .catch(function(err) {
          console.log("An error occurred: " + err);
        });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
 
.camera {
  width: 100vw;
  height: 50vh;
}
</style>
