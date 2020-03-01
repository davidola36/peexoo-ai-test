<template>
    <div class="u-text-center">   
        <input type="file" class="custom-file-input" placeholder="Choose Image" @change="processImage">
        <div class="images-holder">
            <div class="images-cont">
                <img :src="captured" alt="" id = "imageSrc">
            </div>
            <div class="images-cont">
                <canvas id="imageCanvas" v-show="converted"></canvas>
            </div>
        </div>
        <button class="button" @click="greyScale()">Process Image</button>
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
        let imgElement = document.getElementById('imageSrc');
        
        imgElement.onload = function() {
        let image = cv.imread(imgElement);
        cv.imshow('imageCanvas', image);
        image.delete();
        };
    },
    methods: {
        processImage(event) {
             var reader = new FileReader();
             let vm = this
        let imgElement = document.getElementById('imageSrc');

            imgElement.src = URL.createObjectURL(event.target.files[0]);
        },
        greyScale() {

            let srcMat = cv.imread('imageCanvas');
            let displayMat = srcMat.clone();
            let circlesMat = new cv.Mat();
            cv.cvtColor(srcMat, srcMat, cv.COLOR_RGBA2GRAY);
            cv.imshow('imageCanvas', srcMat);
            this.converted = true;

        }
    }
}
</script>

<style>

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