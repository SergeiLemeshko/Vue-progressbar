<template>
  <div class="wrapper">
    <div class="wrapper_title">Введите число от 0 до 1 (прим. 0.3)</div>
    <input v-model="progress" @input="handleInputChange" type="number" min="0" max="1" step="0.1">
    <svg  width="200" height="200">
      <circle cx="100" cy="100" :r="radius" class="circle_loader-top" :stroke-dasharray="circumference"></circle>
      <circle cx="100" cy="100" :r="radius" class="circle_loader-bottom" :stroke-dasharray="circumference" :stroke-dashoffset="addOrReduce"></circle>
    </svg>
    <p class="num">{{ percentLengthFixed * 100 }}%</p>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      radius: 50,//радиус окружности
      progress: 0,//значение вводимое в инпут
      circumference: 0, //длина окружности
    };
  },
  methods: {
    //максимальное и минимальное число для ввода в инпут
    handleInputChange() {
      if (this.progress < 0) {
        return this.progress = 0
      } else if (this.progress >= 1) {
        return this.progress = 1
      } else {
        return this.circumference = 2 * Math.PI * this.radius;//части
      }
    }
  },
  computed: {
    //сдвиг прогрессбара(dash-array) относительно начального положения
    addOrReduce: function() {
      return this.circumference - this.progress * this.circumference;
    },
    //ограничение максимальной длины цифрового индикатора 
    percentLengthFixed: function() {
      let progressFixed = +this.progress;
      return progressFixed.toFixed(2);
    }
  },
  mounted: function(){
    this.handleInputChange();
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background-color: rgb(46, 40, 32);
}

.circle_loader-top {
  fill: none;
  stroke: #362950;
  stroke-width: 10px;
  stroke-linecap: round;
  transition: all 0.2s linear;
  transform-origin: center;
  transform: rotate(-90deg);
  z-index: 1;
}

.wrapper_title {
  display: flex;
  text-align: center;
  margin-top: -40px;
  color: #E3E92B;
}

.circle_loader-bottom {
  fill: none;
  stroke: #E3E92B;
  stroke-width: 10px;
  stroke-linecap: round;
  filter: drop-shadow(0 0mm 1mm rgb(255,255,0));
  transform-origin: center;
  transform: rotate(-90deg);
  transition: stroke-dashoffset 1s;
  z-index: 2;
}

.wrapper {
  position: absolute;
  top: 20%;
  left: 40%;
  width: 200px;
  height: 200px;
  background: #120729;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

input {
  display: block;
  position: absolute;
  margin: 10px 75px;
}

.num {
  position: absolute;
  top: 63px;
  left: 80px;
  width: 45px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgb(255,255,0);
  font-family: "Gill Sans", sans-serif;
  font-size: 18px;
  animation: changeColor 3s infinite;
}

@keyframes changeColor {
  0% {
    color: #E3E92B;
  }
  50% {
    color: #5c5e2d;
  }
  100% {
    color: #E3E92B;
  }
}
</style>
