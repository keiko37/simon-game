<template>
  <div class="simon">
    <h1>Simon Says</h1>
    <div class="simon__content">
      <ul class="game-field">
        <li v-for="circlePart in circleParts"
        :key="circlePart.color"
        :class="[currentValue === circlePart.value ? 'matched' : '', circlePart.color]"
        @click="playerInteract(circlePart.sound, circlePart.value)"
        class="part"
        ></li>
        <div class="round">{{ round }}</div>
      </ul>
      <div class="data-field">
        <button @click="startGame" class="start">СТАРТ</button>
        <h2 v-if="finish">Игра окончена</h2>
        <div v-if="start === false">
          <div class="difficulty">
            <h2>Сложность</h2>
            <input
            v-model="difficultyRange"
            class="difficulty__input"
            type="range"
            id="difficulty"
            name="difficulty"
            min="0"
            max="2">
            <label for="volume">{{ difficulty[difficultyRange].title}}</label>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const sound1 = require('@/assets/sounds/sound1.mp3');
const sound2 = require('@/assets/sounds/sound2.mp3');
const sound3 = require('@/assets/sounds/sound3.mp3');
const sound4 = require('@/assets/sounds/sound4.mp3');

export default {
  name: 'Simon',
  data() {
    return {
      circleParts: [{ color: 'red', sound: sound1, value: 1 },
        { color: 'green', sound: sound2, value: 2 },
        { color: 'blue', sound: sound3, value: 3 },
        { color: 'yellow', sound: sound4, value: 4 }],
      sound: 2,
      difficultyRange: 0,
      difficulty: [{ title: 'Легкая', seconds: 1.5 },
        { title: 'Средняя', seconds: 1 },
        { title: 'Тяжелая', seconds: 0.4 }],
      round: 0,
      start: false,
      finish: false,
      currentValue: 0,
      playerValue: 0,
      gameArray: [],
      roundTry: 0,
    };
  },
  methods: {
    playerInteract(sound, value) {
      this.playSound(sound);
      if (this.start === true) {
        this.playerValue = value;
        const valueFromArray = this.gameArray[this.roundTry];
        if (this.roundTry < this.round && this.playerValue !== valueFromArray) {
          this.finish = true;
          this.finishGame();
          return;
        }
        if (this.roundTry < this.round && this.playerValue === valueFromArray) {
          this.roundTry += 1;
        }
        if (this.roundTry >= this.round) {
          setTimeout(() => { this.roundGame(); }, 2000);
        }
      }
    },
    startGame() {
      this.gameArray = [];
      this.start = true;
      this.round = 0;
      this.finish = false;
      setTimeout(() => { this.roundGame(); }, 2000);
    },
    roundGame() {
      this.round += 1;
      this.roundTry = 0;
      const speed = this.difficulty[Number(this.difficultyRange)].seconds;
      const randomNumber = this.randomInt(1, 5);
      this.gameArray.push(randomNumber);
      this.gameArray.forEach((item, index) => {
        setTimeout(() => {
          switch (item) {
            case 1:
              this.activatePart(sound1, 1);
              break;
            case 2:
              this.activatePart(sound2, 2);
              break;
            case 3:
              this.activatePart(sound3, 3);
              break;
            case 4:
              this.activatePart(sound4, 4);
              break;
            default:
              console.error('Wrong value in gameArray!');
          }
          setTimeout(() => { this.currentValue = 0; }, 200);
        }, (speed * 1000) * index);
      });
    },
    randomInt(minValue, maxValue) {
      const min = Math.ceil(minValue);
      const max = Math.floor(maxValue);
      return Math.floor(Math.random() * (max - min)) + min;
    },
    activatePart(sound, value) {
      this.playSound(sound);
      this.currentValue = value;
    },
    playSound(sound) {
      const targetSound = new Audio(sound);
      targetSound.play();
    },
    finishGame() {
      this.start = false;
      console.log('Game Finished!');
    },
  },
};
</script>

<style lang="scss" scoped>
h1 {
  font-size: 50px;
  margin-bottom: 60px;
}
h2 {
  font-size: 20px;
  margin-bottom: 10px;
}
.simon {
  display: flex;
  flex-direction: column;
  padding: 50px 70px;
  &__content {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
  }
}
.game-field {
  position: relative;
}
.round {
  position: absolute;
  width: 300px;
  height: 300px;
  display: flex;
  justify-content: center;
  font-weight: 600;
  align-items: center;
  background-color: #fff;
  clip-path: circle(25px);
  font-size: 25px;
}
.part {
  width: 300px;
  height: 300px;
  position: absolute;
  border-radius: 50%;
  opacity: 0.6;
  &:active {
    opacity: 1;
    transform: scale(1.015);
  }
}
.red {
  background-color: red;
  clip-path: inset(0 50% 50% 0);
}
.green {
  background-color: green;
  clip-path: inset(0 0 50% 50%);
}
.blue {
  background-color: blue;
  clip-path: inset(50% 0 0 50%);
}
.yellow {
  background-color: yellow;
  clip-path: inset(50% 50% 0 0);
}
.matched {
  opacity: 1;
  transform: scale(1.015);
}
.data-field {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 300px;
  border: 1px solid #ccc;
  border-radius: 20px;
  box-shadow: 2px 2px 5px #000;
  padding: 15px;
  margin: 30px 0 0 250px;
  background-color: #fff;
  & .start {
    width: 130px;
    height: 50px;
    font-size: 20px;
    border-radius: 10px;
    border-bottom: 1px solid #ccc;
    box-shadow: 2px 2px 5px rgb(255, 101, 101);
    background-color: rgb(122, 235, 255);
    color: #fff;
    text-shadow: 1px 1px 3px #000;
    margin-bottom: 20px;
  }
}
.difficulty {
  margin-bottom: 20px;
}

</style>
