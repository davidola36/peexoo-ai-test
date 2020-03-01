<template>
    <div class="u-text-center">   
        <input type="file" class="custom-file-input" placeholder="Choose Image" @change="handleFile">
                <img :src="captured" alt="" id = "imageSrc" v-show="false">


        <div class="images-holder">
            <div class="images-cont" ref="imageCont">
                <canvas id="imageCanvas" ref="imageCanvas" ></canvas>

            </div>
            <div class="images-cont">
            </div>
        </div>
        <button class="button" @click="detect()">Process Image</button>
    </div>
    
</template>

<script>
export default {
    data: () => {
        return {
            captured: '',
            converted: false,
        }
    },
    mounted() {

    },
    methods: {
        triggerFun(event) {
            const loadlModelPromise = cocoSsd.load();
        
            // resolve all the Promises
            Promise.all([loadlModelPromise, this.processImage(event)])
            .then(values => {
            this.detectFromVideoFrame(values[0], this.$refs.imageCanvas);
            })
            .catch(error => {
            console.log(error);
            });
        },
        handleFile(event) {
            this.triggerFun(event)
        },
        processImage(event) {
            return new Promise(resolve => {
                var reader = new FileReader();
                let canvas = document.getElementById('imageCanvas');
                var cxt = canvas.getContext("2d"); 
                let vm = this
                let imgElement = document.getElementById('imageSrc');
                imgElement.src = URL.createObjectURL(event.target.files[0]);
                imgElement.onload = () => {
                    console.log(imgElement.width,imgElement.height, vm.$refs.imageCont.offsetWidth ,'hhdhdh')
                    let height, width
                    if(imgElement.width > vm.$refs.imageCont.offsetWidth  && imgElement.width/vm.$refs.imageCont.offsetWidth > 1) {
                        let remainder = Math.floor(imgElement.width/vm.$refs.imageCont.offsetWidth)
                        height = imgElement.height/remainder
                        width = imgElement.width/remainder
                    }else {
                        height = imgElement.height
                        width = imgElement.width
                    }
                    cxt.drawImage(imgElement,0,0,width,height)
                    // canvas.setAttribute('height',(imgElement.height/10))
                    // canvas.setAttribute('width',(imgElement.width/10))
                }
                // let srcMat = cv.imread('imageCanvas');
                // cv.imshow('imageCanvas', srcMat);
                resolve()
            })
        },
        detectFromVideoFrame(model, video) {
            model.detect(video).then(predictions => {
                console.log('detetcted', predictions)
                this.showDetections(predictions);
            }, (error) => {
            console.log("Couldn't start the webcam")
            console.error(error)
            });
        },
        showDetections(predictions)  {
            const ctx = this.$refs.imageCanvas.getContext("2d");
            // ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
            const font = "24px helvetica";
            ctx.font = font;
            ctx.textBaseline = "top";

            predictions.forEach(prediction => {
            const x = prediction.bbox[0];
            const y = prediction.bbox[1];
            const width = prediction.bbox[2];
            const height = prediction.bbox[3];
            // Draw the bounding box.
            ctx.strokeStyle = "#2fff00";
            ctx.lineWidth = 1;
            ctx.strokeRect(x, y, width, height);
            // Draw the label background.
            ctx.fillStyle = "#2fff00";
            const textWidth = ctx.measureText(prediction.class).width;
            const textHeight = parseInt(font, 10);
            // draw top left rectangle
            ctx.fillRect(x, y, textWidth + 10, textHeight + 10);
            // draw bottom left rectangle
            ctx.fillRect(x, y + height - textHeight, textWidth + 15, textHeight + 10);

            // Draw the text last to ensure it's on top.
            ctx.fillStyle = "#000000";
            ctx.fillText(prediction.class, x, y);
            ctx.fillText(prediction.score.toFixed(2), x, y + height - textHeight);
            });
        }
    }
}
</script>

<style>


#imageCanvas {
    /* width: 100%;
    min-height: 100px; */
    /* max-height: 100%; */
    /* object-fit: contain; */
}
.images-holder {
    margin-top: 2rem;
    margin-bottom: 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 30rem;
    padding: 0 2rem;
}

.images-cont {
    height: 100%;
    width: 45%;
    border: 1px solid #494949;
    border-style: dashed;
    display: flex;
    align-items: center;
}

.images-cont img {
    width: 100%;
    height: auto;
    max-height: 100%;
    object-fit: contain;
}

.button {
  background: #FFB503;
  border-radius: 3px;
  padding: 15px 30px;
  border-radius: 10px;
  white-space: nowrap;
  -webkit-user-select: none;
  cursor: pointer;
  font-size: 16px;
  border: none;
}
</style>