<template>
  <div>
          <!-- <embed src="/WithdrawnVsBlock.text" loop="false" autostart="true" width="2" height="0"> -->

    <HelloWorld ></HelloWorld>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue' ;
import 'bootstrap' ;
import 'bootstrap/dist/css/bootstrap.min.css'
export default {
  components: {
    HelloWorld
  }
}
</script>

<style>



::-webkit-scrollbar {
  width: 20px;
}

::-webkit-scrollbar-track {
  background-color: transparent;
}

::-webkit-scrollbar-thumb {
  background-color: #d6dee1;
  border-radius: 20px;
  border: 6px solid transparent;
  background-clip: content-box;
}

::-webkit-scrollbar-thumb:hover {
  background-color: #a8bbbf;
}

</style>
