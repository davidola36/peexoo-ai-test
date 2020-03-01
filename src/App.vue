<template>
  <div>
    <header class="header">
      <h2>Peexoo AI Test</h2>
    </header>
    <div class="panel">
      <p v-for="(item,i) in tabs" :key="i" :class="{'active': chosen == i}" @click="chosen = i"> {{item}}</p>

    </div>
    <greyscale v-if="chosen == 0" />
    <Fetch v-if="chosen == 1" />
    <Detect v-if="chosen == 2"/>
    <Writeup v-if="chosen == 3" />

  </div>
</template>

<script>
import greyscale from './components/greyscale'
import Fetch from './components/fetch'
import Detect from './components/detect'
import Writeup from './components/writeup'
export default {
  name: 'App',
  data: () => {
    return {
      tabs: ['Create greyscale', 'Fetch and store', 'Detect fruits', 'Write up'],
      chosen: 2
    }
  },
  components: {
    greyscale,
    Fetch,
    Detect,
    Writeup
  }
}
</script>

<style>

  .panel {
    display: flex;
    align-items: center; 
    justify-content: space-between;
    padding: 0 2rem;
    font-size: 24px;
    margin-bottom: 2rem;
  }

  .panel p {
    cursor: pointer;
  }

  .panel .active {
    color: #FFB503;
  }

  body {
    margin: 0;
    padding: 0;
  }

  * {
    margin: 0;
    padding: 0;
  }

  html {
    font-size: 70%;
  }

  .header {
    height: 7rem;  
    background-color: #FFB503;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #FFFFFF;
    font-size: 3rem;
    margin-bottom: 2rem;
  }

  .u-text-center {
    text-align: center;
  }

  .custom-file-input {
  color: transparent;
}
.custom-file-input::-webkit-file-upload-button {
  visibility: hidden;
}
.custom-file-input::before {
  content: 'Choose image';
  color: black;
  display: inline-block;
  background: #FFB503;
  /* border: 1px solid #999; */
  border-radius: 3px;
  padding: 15px 30px;
  border-radius: 10px;
  outline: none;
  white-space: nowrap;
  -webkit-user-select: none;
  cursor: pointer;
  /* text-shadow: 1px 1px #fff; */
  font-size: 16px;
}
.custom-file-input:hover::before {
    background: #FFB503;

}
.custom-file-input:active {
  outline: 0;
}
.custom-file-input:active::before {
  background: #FFB503;

}

/* display loading gif and hide webpage */
.modal {
    display:    none;
    position:   fixed;
    z-index:    1000;
    top:        0;
    left:       0;
    height:     100%;
    width:      100%;
    background: rgba( 255, 255, 255, .8)
                url('http://i.stack.imgur.com/FhHRx.gif')
                50% 50%
                no-repeat;
}

/* prevent scrollbar from display during load */
body.loading {
    overflow: hidden;
}

/* display the modal when loading class is added to body */
body.loading .modal {
    display: block;
}


</style>
