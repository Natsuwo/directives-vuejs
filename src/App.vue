<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 offset-sm-2 col-md-6 offset-md-3">
        <h1>Default/ Built-in Directives</h1>
        <p v-text="'something-else'"></p>
        <p v-html="'<h1>this h1 tag</h1>'"></p>
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12 col-sm-8 offset-sm-2 col-md-6 offset-md-3">
        <h1>Custom Directives</h1>
        <p v-custom:background.delayed="'lightgreen'">Color style by directives global</p>
        <p v-local-custom:background.delayed.blink="{mainColor: 'red', secondColor: 'green', delay: 500}">Color style by directives local</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  directives: {
    "local-custom": {
      bind(el, binding, vnode) {
        var delay = 0;
        if (binding.modifiers["delayed"]) {
          delay = 3000;
        }

        if (binding.modifiers["blink"]) {
          let mainColor = binding.value.mainColor;
          let secondColor = binding.value.secondColor;
          let currentColor = mainColor;
          setTimeout(() => {
            setInterval(() => {
              currentColor == secondColor
                ? (currentColor = mainColor)
                : (currentColor = secondColor);
              setTimeout(() => {
                if (binding.arg == "background") {
                  el.style.backgroundColor = currentColor;
                } else {
                  el.style.color = currentColor;
                }
              }, delay);
            }, binding.value.delay);
          }, delay);
        } else {
          setTimeout(() => {
            if (binding.arg == "background") {
              el.style.backgroundColor = binding.value.mainColor;
            } else {
              el.style.color = binding.value.mainColor;
            }
          }, delay);
        }
      }
    }
  }
};
</script>

<style>
</style>
