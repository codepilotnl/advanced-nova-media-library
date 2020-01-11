<template>
  <div class="image-focus">
    <div class="grid">
      <div class="focused-image-container top-left">
        <img
          ref="top_left"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <div class="focused-image-container top-center">
        <img
          ref="top_center"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <div class="focused-image-container top-right">
        <img
          ref="top_right"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <div class="focused-image-container center-left">
        <img
          ref="center_left"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <div class="focused-image-container center-center">
        <img
          ref="center_center"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <div class="focused-image-container center-right">
        <img
          ref="center_right"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <div class="focused-image-container bottom-left">
        <img
          ref="bottom_left"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <div class="focused-image-container bottom-center">
        <img
          ref="bottom_center"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <div class="focused-image-container bottom-right">
        <img
          ref="bottom_right"
          class="focused-image"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
    </div>
    <div class="image-focus-picker-container">
      <h3 class="instruction mb-2">
        {{ __('Drag to select focus') }}
      </h3>
      <div>
        <img
          ref="picker"
          id="focus-picker-img"
          :src="image.__media_urls__.preview"
          alt=""
          :data-focus-x="focusSettings.focus.x"
          :data-focus-y="focusSettings.focus.y"
        />
      </div>
      <button
        type="button"
        class="form-file-btn btn btn-default btn-primary mt-2"
        @click.prevent="handleSetFocus"
      >
        {{ __('Set Focus') }}
      </button>
    </div>
  </div>
</template>

<script>
import { FocusedImage, FocusPicker, Focus } from 'image-focus'

export default {
  props: {
    image: { type: Object, required: true }
  },
  data() {
    return {
      focusSettings: {
        focus: {
          x: 0,
          y: 0
        },
        debounceTime: 17,
        updateOnWindowResize: true
      },
      focusImages: []
    }
  },
  mounted() {
    if ('focus' in this.image.custom_properties) {
      this.focusSettings.focus.x = this.image.custom_properties.focus.x
      this.focusSettings.focus.y = this.image.custom_properties.focus.y
    }

    this.focusImages.push(
      new FocusedImage(this.$refs.top_left, this.focusSettings)
    )
    this.focusImages.push(
      new FocusedImage(this.$refs.top_center, this.focusSettings)
    )
    this.focusImages.push(
      new FocusedImage(this.$refs.top_right, this.focusSettings)
    )
    this.focusImages.push(
      new FocusedImage(this.$refs.center_left, this.focusSettings)
    )
    this.focusImages.push(
      new FocusedImage(this.$refs.center_center, this.focusSettings)
    )
    this.focusImages.push(
      new FocusedImage(this.$refs.center_right, this.focusSettings)
    )
    this.focusImages.push(
      new FocusedImage(this.$refs.bottom_left, this.focusSettings)
    )
    this.focusImages.push(
      new FocusedImage(this.$refs.bottom_center, this.focusSettings)
    )
    this.focusImages.push(
      new FocusedImage(this.$refs.bottom_right, this.focusSettings)
    )

    let focus = this.focusSettings.focus
    const focusPicker = new FocusPicker(this.$refs.picker, {
      focus,
      onChange: newFocus => {
        this.focusSettings.focus.x = newFocus.x
        this.focusSettings.focus.y = newFocus.y
        this.focusImages.forEach(focusedImage =>
          focusedImage.setFocus(newFocus)
        )
      }
    })
  },
  methods: {
    handleSetFocus() {
      this.$emit('focus-completed', {
        image: this.image,
        focus: this.focusSettings.focus
      })
      this.$emit('close')
    }
  }
}
</script>

<style lang="scss">
.image-focus {
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  min-height: 100%;
  background-color: white;
  z-index: 1000;
  .grid {
    display: grid;
    display: -ms-grid;
    grid-template-columns: 6fr 3fr 2fr;
    -ms-grid-columns: 6fr 3fr 2fr;
    grid-template-rows: 6fr 3fr 2fr;
    -ms-grid-rows: 6fr 3fr 2fr;
    grid-gap: 5px;

    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    /* needed for safari */
    height: 100%;
  }
  .top-left {
    -ms-grid-column: 1;
    -ms-grid-row: 1;
  }

  .top-center {
    -ms-grid-column: 2;
    -ms-grid-row: 1;
  }

  .top-right {
    -ms-grid-column: 3;
    -ms-grid-row: 1;
  }

  .center-left {
    -ms-grid-column: 1;
    -ms-grid-row: 2;
  }

  .center-center {
    -ms-grid-column: 2;
    -ms-grid-row: 2;
  }

  .center-right {
    -ms-grid-column: 3;
    -ms-grid-row: 2;
  }

  .bottom-left {
    -ms-grid-column: 1;
    -ms-grid-row: 3;
  }

  .bottom-center {
    -ms-grid-column: 2;
    -ms-grid-row: 3;
  }

  .bottom-right {
    -ms-grid-column: 3;
    -ms-grid-row: 3;
  }
  .focused-image {
    /* Just animating transitions */
    transition: top 0.25s ease-in-out, left 0.25s ease-in-out;
  }
  .image-focus-picker-container {
    position: absolute;
    top: 30px;
    left: 30px;
    padding: 15px;
    width: 300px;
    background-color: ghostwhite;
    border-radius: 4px;
  }
  #focus-picker-img {
    max-width: 100%;
  }
  .form-group {
    display: block;
    margin-top: 12px;
  }
  .form-group input {
    width: 100%;
    font-size: 14px;
    font-family: monospace;
    box-sizing: border-box;
  }
  .instruction {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 0;
  }
  .github-link {
    display: flex;
  }
  img {
    max-width: none;
  }
}
</style>
