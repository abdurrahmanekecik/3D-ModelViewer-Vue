<template>
  <div class="d-flex" id="wrapper">
    <!-- Sidebar-->
    <div class="border-end bg-white" id="sidebar-wrapper">
      <div class="sidebar-heading border-bottom bg-light">Düzenleme Menüsü</div>
      <div class="list-group list-group-flush">
        <div class="list-group-item list-group-item-action list-group-item-light p-3">
          <p>Dorse Kapağı Renk</p>
          <button class="btn btn-primary" @click="function1('blue')">
            Mavi
          </button>
          <button class="btn btn-warning" @click="function1('yellow')">
            Sarı
          </button>
          <button class="btn btn-success" @click="function1('green')">
            Yeşil
          </button>
        </div>

        <div class="list-group-item list-group-item-action list-group-item-light p-3">
          <p>Dorse Gövde Rengi</p>
          <button class="btn btn-primary" @click="function2('blue')">
            Mavi
          </button>
          <button class="btn btn-warning" @click="function2('yellow')">
            Sarı
          </button>
          <button class="btn btn-success" @click="function2('green')">
            Yeşil
          </button>
        </div>

        <div class="list-group-item list-group-item-action list-group-item-light p-3">
          <p>Varyantlar</p>
          <select v-if="variants.length > 0" @input="changeVariant">
            <option v-for="variant in variants" :value="variant.value">
              {{ variant.text }}
            </option>
          </select>
        </div>

        <div class="list-group-item list-group-item-action list-group-item-light p-3">
          <p>Tekerlek</p>
          <button class="btn btn-primary" @click="function3(1)">Kaldır</button>
          <button class="btn btn-success" @click="function3(0)">İndir</button>
        </div>
      </div>
    </div>
    <!-- Page content wrapper-->
    <div id="page-content-wrapper">
      <main>
        <div class="container-fluid">
          <h1 class="mt-4">Dorse Düzenleme</h1>

          <model-viewer id="color" ref="modelViewer" @load="onModelLoad" class="trailer" src="/public/yap.glb" ar
            ar-modes="webxr scene-viewer quick-look" interaction-prompt="none" camera-controls tone-mapping="commerce"
            shadow-intensity="1">
          </model-viewer>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      variants: [],
    };
  },
  methods: {
    async function1(color) {
      await this.$refs.modelViewer.updateComplete;
      const modelViewerColor = this.$refs.modelViewer;
      const [material] = modelViewerColor.model.materials;

      if (color === "blue") {
        material.pbrMetallicRoughness.setBaseColorFactor("#0d6efd");
      } else if (color === "yellow") {
        material.pbrMetallicRoughness.setBaseColorFactor("#ffc107");
      } else {
        material.pbrMetallicRoughness.setBaseColorFactor("#198754");
      }
    },

    async function2(color) {
      await this.$refs.modelViewer.updateComplete;
      const model = this.$refs.modelViewer;
      const [material] = model.model.materials;

      if (color === "blue") {
        const variantName = "govde-siyah";
        model.model.variantName = variantName;
        console.log(model.model.variantName);
        console.log(model.availableVariants);
        console.log(this.variants);
        console.log(this.names);
      } else if (color === "yellow") {
        material.pbrMetallicRoughness.setBaseColorFactor("#ffc107");
      } else {
        material.pbrMetallicRoughness.setBaseColorFactor("#198754");
      }
    },
    async function3(value) {
      await this.$refs.modelViewer.updateComplete;
      const model = this.$refs.modelViewer;

      if (value === 1) {
        model.setAttirbute(
          "animation-name",
          "Wheel_trailer02|Take 001|BaseLayer"
        );
        model.timeScale = 1;
        model.play({ repetitions: 1 });
        alreadyPlay = true;
      } else {
        model.setAttribute(
          "animation-name",
          "Wheel_trailer02|Take 001|BaseLayer"
        );
        model.timeScale = -1;
        model.play({ repetitions: 1 });
        alreadyPlay = false;
      }
    },

    changeVariant(event) {
      this.$refs.modelViewer.variantName =
        event.target.value === "default" ? null : event.target.value;
    },
  },

  mounted() {
    this.$refs.modelViewer.addEventListener("load", () => {
      this.variants = this.$refs.modelViewer.availableVariants.map(
        (variant) => ({
          value: variant,
          text: variant,
        })
      );
    });
  },
};
</script>

<style>
.trailer {
  width: 800px;
  height: 600px;
}
</style>
