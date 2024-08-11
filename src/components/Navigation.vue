<script setup>
const props = defineProps({
  locations: Array,
});
const emit = defineEmits(['moveUp', 'moveDown']);

function shiftUpHandler(index) {
  emit('moveUp', index);
}

function shiftDownHandler(index) {
  emit('moveDown', index);
}
</script>

<template>
  <div class="navigation-card">
    <div class="content">
      <ul enter-class="location-list">
        <li v-for="(item, index) in props.locations" :key="index" class="location-list__item">
          <span class="dot" v-if="index < locations.length - 1">
            <img src="/dot.svg" alt="" />
            <span class="ellipsis"><img src="/ellipsis.svg" alt=""></span>
          </span>
          <span class="dot" v-else>
            <img src="/location-icon.svg" alt="" />
          </span>
          <span class="text">{{ item }}</span>
          <span class="arrows">
            <span class="icon up" @click="shiftUpHandler(index)" v-show="index > 0">
              <img src="/arrow-up.svg" alt="Move Up" />
            </span>
            <span class="icon down" @click="shiftDownHandler(index)" v-show="index < props.locations.length - 1">
              <img src="/arrow-down.svg" alt="Move Down" />
            </span>
          </span>
        </li>

        <!-- Use this li for rendering each location item as it contains all the data-testid attributes required for the tests to pass-->
        <!-- <li :key="'row' + index" :data-testid="'location-' + index"
        class="layout-row justify-content-between align-items-center mr-8 pl-40 relative">
        <div class="layout-column justify-content-start align-items-center handle">
        <i :class="this.getClasses('marker', index)">
          {{ this.isLast(index) ? 'room' : 'radio_button_checked' }}</i>
        <i :class="this.getClasses('dots', index)">more_vert</i>
        </div>
        <div class="location-name">
          <p class="caption text-start mb-4" data-testid="location">{{ location }}</p>
          </div>
          <div>
          <button class="icon-only small mx-0" data-testid="up-button">
            <i class="material-icons">arrow_upward</i>
          </button>
          <button class="icon-only small mx-0" data-testid="down-button">
            <i class="material-icons">arrow_downward</i>
          </button>
        </div> -->
        <!-- </li> -->
      </ul>
    </div>
    <div class="map">
      <img src="/map.svg" class="fill" alt="map" />
    </div>
  </div>
</template>
<style scoped>
  .navigation-card {
    width: 100%;
    background-color: #ffffff;
    max-width: 1000px;
    display: grid;
    grid-template-columns: 1fr minmax(200px, 400px);
  }
  .map {
    width: 100%;
    height: 100%;
    overflow: hidden;
    img {
      display: block;
      width: 100%;
      height: 100%;
      max-width: 100%;
      object-fit: cover;
    }
  }
  .content {
    padding: 1.5vw;
  }
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
    width: 100%;
    display: grid;

  }
  .location-list {}
  .location-list__item {
    --list-height: 45px;
    width: 100%;
    height: 45px;
    display: flex;
    align-items: center;
    gap: 10px;  
    .text {
      border-block-end: 1px solid #ccc; 
    };
    .arrows {
      display: flex;
      flex-shrink: 0;
      gap: 0.1rem;
      margin-inline-start: auto;
      .icon {
        width: var(--list-height);
        height: var(--list-height);
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        border-radius: 0.25rem;
        &:hover {
          background-color: #f5f5f5;
        }
      }
    }
    .dot {
      position: relative;
      display: flex;
      justify-content: center;
  
      .ellipsis {
        position: absolute;
        inset-block-start: calc(100% + 2px);
  
        img {
          width: 10px;
          height: 25px;
        }
      }
    }
    img,
    svg {
      display: block;
      /* max-width: 15px; */
      /* width: 15px; */
    }
  }

</style>
