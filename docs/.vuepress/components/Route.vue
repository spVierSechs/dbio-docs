<template>
  <div :id="url">
    <span :class="methodClass">{{method.toUpperCase()}}</span>
    <span class="path" v-html="formattedPath"></span>
  </div>
</template>

<script>
export default {
  props: ["method", "path", "partparams"],
  computed: {
    methodClass() {
      return `method method-${this.method.toLowerCase()}`;
    },
    formattedPath() {
      if (this.partparams)
        return this.path.replace(
          /:.+?((?=\/))/g,
          x => `<span class="param">${x}</span>`
        );
      else
        return this.path.replace(
          /:.+/g,
          x => `<span class="param">${x}</span>`
        );
    },
    url() {
      return this.path.replace(/[:\/]/g, "").toLowerCase();
    }
  }
};
</script>

<style>
.method,
.path {
  font-size: 15pt;
}

.path {
  font-family: "Consolas", monospace;
}

.method {
  font-weight: bold;
  padding: 5px;
  padding-right: 0.5em;
  margin-right: 10px;
}

.method-get {
  background-color: rgb(20, 170, 95);
  color: white;
}

.method-post {
  background-color: rgb(20, 103, 170);
  color: white;
}

.method-put {
  background-color: rgb(134, 62, 228);
  color: white;
}

.method-delete {
  background-color: rgb(228, 62, 84);
  color: white;
}

.param {
  color: rgb(20, 170, 95);
}
</style>