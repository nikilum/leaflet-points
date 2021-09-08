<template>
  <div id="app">
    <l-map class="map" :zoom="zoom" :center="center" ref="map">
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker :lat-lng="[point.latitude, point.longitude]" v-for="point of points" :key="point.id"/>
    </l-map>
    <transition name="fade">
      <button class="btn control-panel__open-button" @click="openPointsList" v-if="!pointsListOpened">Список меток</button>
      <div class="control-panel" v-if="pointsListOpened">
        <div class="control-panel__inner-container">

          <div class="flex-column">
            <label for="pointFinderInput">Поиск по названию метки</label>
            <input class="control-panel__input" id="pointFinderInput" v-model="pointFinder">
          </div>

          <div class="control-panel__points-list">
            <div tabindex="0" class="control-panel__point" v-for="point of filteredPointsList" :key="point.id" @click="onPointClicked(point)" :class="{'active-point': activePointId === point.id}">
              {{ point.name }}
            </div>
          </div>

          <div>
            <button class="btn btn-sm control-panel__close_button" @click="hidePointsList">Закрыть</button>
          </div>

        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import { Icon } from 'leaflet';

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});

export default {
  name: 'App',
  data() {
    return {
      pointFinder: '',
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      zoom: 15,
      activePointId: undefined,
      center: [51.505, -0.159],
      pointsListOpened: false,
      points: [
        {
          "id": 1,
          "latitude": 50.760918,
          "longitude": 4.110170,
          "name": "ВАЗ"
        },
        {
          "id": 2,
          "latitude": 47.492647,
          "longitude": 19.051399,
          "name": "ГАЗель"
        },
        {
          "id": 3,
          "latitude": 41.902689,
          "longitude": 12.496176,
          "name": "Lexus"
        },
        {
          "id": 4,
          "latitude": 43.779787,
          "longitude": 11.265817,
          "name": "Volkswagen"
        },
        {
          "id": 5,
          "latitude": 52.373057,
          "longitude": 4.892557,
          "name": "Lada"
        },
        {
          "id": 6,
          "latitude": -22.903150,
          "longitude": -43.189903,
          "name": "Kia"
        },
        {
          "id": 7,
          "latitude": 38.716174,
          "longitude": -9.141589,
          "name": "Bentley"
        },
        {
          "id": 8,
          "latitude": 50.080293,
          "longitude": 14.428983,
          "name": "Porsche"
        },
        {
          "id": 9,
          "latitude": 48.856663,
          "longitude": 2.351556,
          "name": "BMW"
        },
        {
          "id": 10,
          "latitude": 45.438095,
          "longitude": 12.319029,
          "name": "Honda"
        }
      ]
    }
  },
  methods: {
    openPointsList() {
      this.pointsListOpened = true
    },
    hidePointsList () {
      this.pointsListOpened = false
    },
    onPointClicked(point) {
      this.activePointId = point.id
      this.$refs.map.mapObject.setView([point.latitude, point.longitude], 5);
    }
  },
  computed: {
    filteredPointsList() {
      return this.points.filter(point => point.name.indexOf(this.pointFinder) !== -1)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  width: 100vw;
  height: 100vh;
}

.map {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
}

.control-panel__open-button {
  position: fixed;
  top: 1rem;
  right: 1rem;
  z-index: 1000;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}

.btn {
  padding: 15px 20px;
  border-radius: 4px;
  border: none;
  background: rgb(26, 115, 232);
  color: white;
  cursor: pointer;
  transition: 100ms linear;
}

.btn:hover {
  background: rgb(49, 131, 239);
}

.btn:active {
  background: rgb(23, 97, 195);
}

.control-panel {
  z-index: 1001;
  background: white;
  width: 300px;
  padding: 1rem;
  height: 90%;
  position: absolute;
  top: 1rem;
  right: 1rem;
  border-radius: 10px;
  box-shadow: 1px 3px 10px darkgray;
}

@media (max-width: 370px) {
  .control-panel {
    width: 90%;
    right: 0;
  }
}

.control-panel__input {
  margin-top: 5px;
  padding: 7px 10px;
  border-radius: 4px;
  outline: none !important;
  border: 1px solid lightgray;
  margin-bottom: 1rem;
}

.control-panel__inner-container {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: column;
}

.control-panel__points-list {
  width: 100%;
  display: flex;
  flex-grow: 1;
  flex-direction: column;
  border: 1px solid lightgray;
  border-radius: 4px;
  margin-bottom: 1rem;
  overflow: auto;
}

.control-panel__point {
  padding: .5rem;
  transition: 75ms ease-in-out;
  cursor: pointer;
}

.control-panel__point:hover, .active-point {
  background: lightblue;
}

.btn-sm {
  padding: 5px 8px;
}

.flex-column {
  display: flex;
  flex-direction: column;
}
</style>
