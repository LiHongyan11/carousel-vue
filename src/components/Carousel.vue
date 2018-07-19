<template>
  <div class="box">
    <span class="prev arrow" @click="handlePrev"></span>
    <span class="next arrow" @click="handleNext"></span>
    <transition-group name="list" class="list" tag="ul" :style="`height: ${itemOptions.maxSize}px`">
      <li
        v-for="(item, index) in list"
        :key="index"
        :style="slideStyle[index]">
        <img :src="item" :style="`border-radius: ${borderRadius};`"></li>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: 'Carousel',
  props: {
    list: {
      type: Array,
      default() {
        return [];
      }
    },
    // shrink: {
    //   type: Number,
    //   default: 0.8
    // },
    borderRadius: {
      type: String,
      default: '15px'
    },
    // showNum: {
    //   type: Number,
    //   default: 7,
    // }
  },
  data() {
    return {
      itemOptions: {
        maxSize: '100px',
        scaling: 0.8,
        showNum: 7
      },
      activeIndex: 3,
      showList: []
    }
  },

  mounted() {
    const ulWidth = document.querySelector('.list').clientWidth;
    this.itemOptions.maxSize = Math.floor((ulWidth - (this.itemOptions.showNum - 1) * 5) / 4.9);
    this.updateShowList();
  },

  computed: {
    slideStyle() {
      let styles = [];
      this.showList.map((item, idx) => {
        if (!item) {
          styles[idx] = {
            'visibility': 'hidden',
            'width': 0,
            'height': 0,
            'margin-right': 0
          };
        } else {
          styles[idx] = {
            'visibility': 'visible',
            'width': this.showList[idx] + 'px',
            'height': this.showList[idx] + 'px',
            'margin-right': '5px'
            // 'transform': `scale(${this.showList[idx]})`,
          };
        }
      });
      return styles;
    }
  }, 

  methods: {
    updateShowList() {
      const { scaling, maxSize } = this.itemOptions;
      this.showList.splice(this.list.length);
      this.showList.fill(0);
      this.$set(this.showList, this.activeIndex, maxSize);
      this.$set(this.showList, this.activeIndex - 1, Math.floor(maxSize * scaling));
      this.$set(this.showList, this.activeIndex + 1, Math.floor(maxSize * scaling));
      this.$set(this.showList, this.activeIndex - 2, Math.floor(maxSize * scaling * scaling));
      this.$set(this.showList, this.activeIndex + 2, Math.floor(maxSize * scaling * scaling));
      this.$set(this.showList, this.activeIndex - 3, Math.floor(maxSize * scaling * scaling * scaling));
      this.$set(this.showList, this.activeIndex + 3, Math.floor(maxSize * scaling * scaling * scaling));
      this.$forceUpdate();
    },
    handleNext() {
      if (this.activeIndex + 1 >= this.list.length - 4) {
        return false;
      } else {
        this.activeIndex = this.activeIndex + 1;
        this.updateShowList();
      }
    },
    handlePrev() {
      if (this.activeIndex - 1 <= 3) {
        return false;
      } else {
        this.activeIndex = this.activeIndex - 1;
        this.updateShowList();
      }
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  margin: 0;
  padding: 0;
}
.box {
  position: relative;
  padding: 0 20px;
}
.arrow {
  position: absolute;
  border: 6px solid transparent;
  border-top-color: #bbb;
  top: 50%;
  margin-top: -3px;
}
.prev {
  left: 0;
  transform: rotate(90deg);
}
.next {
  right: 0;
  transform: rotate(-90deg);
}
ul {
  list-style: none;
  overflow: hidden;
  display: flex;
  align-items: center;
}
li {
  flex: 0 0 auto;
  width: 0;
  height: 0;
  transition: all 1s;
  
}
li:first-child {
  margin-left: 0;
}
img {
  width: 100%;
  height: 100%;
}
</style>

