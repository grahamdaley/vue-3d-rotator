<template>
  <div class="rotator">
    <div class="nav l">
      <i @click="navPrevious" class="fas fa-chevron-circle-left" />
    </div>
    <div class="figure">
      <slot></slot>
    </div>
    <div class="nav r">
      <i @click="navNext" class="fas fa-chevron-circle-right" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted } from "vue";
import Hammer, { DIRECTION_LEFT, DIRECTION_RIGHT } from "hammerjs";
import HammerInput from "hammerjs";

export default defineComponent({
  name: "Rotator",
  props: {
    touchEvents: {
      type: Boolean,
      default: true,
    },
    gap: {
      type: Number,
      default: 0,
    },
    bfc: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    let theta = 0.0;
    let currImage = 0;
    let figure: HTMLElement | null | undefined = null;
    let images: HTMLCollection | null | undefined = null;

    onMounted(() => {
      const root = document.querySelector(".rotator");
      figure = root?.querySelector(".figure");
      images = figure?.children;
      const n = images ? images.length : 0;
      const imageWidth = images
        ? parseFloat(getComputedStyle(images[0]).width)
        : 0.0;
      theta = (2 * Math.PI) / n;
      setupCarousel(n, imageWidth);
      window.addEventListener("resize", () => {
        setupCarousel(n, imageWidth);
      });

      if (root && props.touchEvents) {
        const hammer = new Hammer(root as HTMLElement);
        hammer.on("panend", (event: HammerInput) => {
          if (event.direction == DIRECTION_LEFT) {
            navNext();
          } else if (event.direction == DIRECTION_RIGHT) {
            navPrevious();
          }
        });
      }
    });

    const navNext = () => {
      currImage++;
      rotateCarousel(currImage);
    };

    const navPrevious = () => {
      currImage--;
      rotateCarousel(currImage);
    };

    const setupCarousel = (n: number, width: number) => {
      var apothem = width / (2 * Math.tan(Math.PI / n));
      if (figure) figure.style.transformOrigin = `50% 50% ${-apothem}px`;

      if (images) {
        for (var i = 0; i < n; i++)
          (images[i] as HTMLElement).style.padding = `${props.gap}px`;
        for (i = 1; i < n; i++) {
          const image = images[i] as HTMLElement;
          image.style.transformOrigin = `50% 50% ${-apothem}px`;
          image.style.transform = `rotateY(${i * theta}rad)`;
        }
        if (props.bfc)
          for (i = 0; i < n; i++)
            (images[i] as HTMLElement).style.backfaceVisibility = "hidden";
      }

      rotateCarousel(currImage);
    };

    const rotateCarousel = (imageIndex: number) => {
      if (figure) figure.style.transform = `rotateY(${imageIndex * -theta}rad)`;
    };

    return {
      navNext,
      navPrevious,
    };
  },
});
</script>
<style lang="scss">
// Carousel configuration parameters
$item-separation: 0px;
$viewer-distance: 500px;

.rotator {
  padding: 20px;
  perspective: $viewer-distance;
  overflow: hidden;
  display: flex;
  justify-content: center;

  .figure {
    margin: 0;
    width: auto;
    height: auto;
    align-self: center;
    transform-style: preserve-3d;
    transition: transform 0.5s;

    .rotateItem {
      box-sizing: border-box;
      padding: 0 calc($item-separation / 2);

      &:not(:first-of-type) {
        position: absolute;
        left: 0;
        top: 0;
      }
    }
  }

  .nav {
    width: 40px;
    height: auto;
    align-self: stretch;
    i {
      font-size: 36px;
      position: relative;
      top: 50%;
      -ms-transform: translateY(-50%);
      -webkit-transform: translateY(-50%);
      transform: translateY(-50%);
      cursor: pointer;
    }
  }
  .nav.r {
    padding-left: 4px;
  }
}
</style>
