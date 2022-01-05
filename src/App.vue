<template>
  <div class="App">
    <header class="header">
      <div class="title-container">
        <h1 class="nav-title">Snake Game</h1>
      </div>
      <Navigation :difficulty="difficulty" :length="body.length" :updateDifficulty="updateDifficulty" />
    </header>
    <main class="main">
      <Field :fields="fields" />
    </main>
    <footer class="footer">
      <Button @onChangeStatus="updateStatus" :status="status" />
      <ManipulationPanel @onChange="onChangeDirection" />
    </footer>
  </div>
</template>

<script>
import Navigation from "./components/Navigation";
import Field from "./components/Field";
import Button from "./components/Button";
import ManipulationPanel from "./components/ManipulationPanel";
import { initFields, getFoodPosition } from "./utils";
import {
  GameStatus,
  Direction,
  OppositeDirection,
  Delta,
  DirectionKeyCodeMap,
  defaultFieldSize,
  initialBody,
  initialValues,
  defaultDifficulty,
  Difficulty,
} from "./constants";

export default {
  name: "App",
  components: {
    Navigation,
    Field,
    Button,
    ManipulationPanel,
  },
  data() {
    return {
      timer: undefined,
      fields: initialValues,
      status: GameStatus.init,
      body: [initialBody],
      direction: Direction.up,
      difficulty: defaultDifficulty,
    };
  },
  methods: {
    playGame() {
      if (this.status !== GameStatus.playing) {
        return false;
      }
      const interval = Difficulty[this.difficulty - 1];
      this.timer = setInterval(() => {
        const newPosition = {
          x: this.body[0].x + Delta[this.direction].x,
          y: this.body[0].y + Delta[this.direction].y,
        };
        if (this.isCollision(newPosition)) {
          this.status = GameStatus.gameover;
        } else {
          const newBody = [...this.body];
          if (this.fields[newPosition.y][newPosition.x] === "food") {
            const newFood = getFoodPosition(defaultFieldSize, this.body);
            this.fields[newFood.y][newFood.x] = "food";
          } else {
            const remove = newBody.pop();
            this.fields[remove.y][remove.x] = "";
          }
          this.fields[newPosition.y][newPosition.x] = "snake";
          newBody.unshift(newPosition);
          this.body = newBody;
        }
      }, interval);
    },
    onChangeDirection(newDirection) {
      if (this.status !== GameStatus.playing) {
        return;
      }
      if (OppositeDirection[this.direction] === newDirection) {
        return;
      }
      this.direction = newDirection;
    },
    updateDifficulty(difficulty) {
      if (this.status !== GameStatus.init) {
        return;
      }
      if (difficulty < 1 || difficulty > Difficulty.length) {
        return;
      }
      this.difficulty = difficulty;
    },
    updateStatus(status) {
      this.status = status;
    },
    isCollision(position) {
      if (position.y < 0 || position.x < 0) {
        return true;
      }
      if (position.y > defaultFieldSize - 1 || position.x > defaultFieldSize - 1) {
        return true;
      }
      return false;
    },
    isEatingMyself(position) {
      return this.fields[position.y][position.x] === "snake";
    },
  },
  watch: {
    // body: {
    //   handler(next, prev) {
    //     this.fields[prev[0].y][prev[0].x] = "";
    //     this.fields[next[0].y][next[0].x] = "snake";
    //   },
    //   deep: true,
    // },
    status(newStatus) {
      switch (newStatus) {
        case GameStatus.init:
          this.body = [initialBody];
          this.direction = Direction.up;
          this.fields = initFields(defaultFieldSize, initialBody, getFoodPosition(defaultFieldSize, [initialBody]));
          break;
        case GameStatus.gameover:
          clearInterval(this.timer);
          break;
        case GameStatus.playing:
          this.playGame();
          break;
        case GameStatus.suspended:
          clearInterval(this.timer);
          break;
        default:
          return;
      }
    },
  },
  created() {
    const handleKeyDown = (e) => {
      const newDirection = DirectionKeyCodeMap[e.keyCode];
      if (!newDirection) {
        return;
      }
      this.onChangeDirection(newDirection);
    };
    document.addEventListener("keydown", handleKeyDown);
  },
};
</script>

<style>
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans",
    "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.is-hidden {
  visibility: hidden !important;
}

/* borderを含めた幅で要素の幅が計算されるように */
*,
*:before,
*:after {
  box-sizing: border-box;
}

.App {
  max-width: calc(9px * 35 + 1px * 2); /* a. (マスの幅) * 個数 + (ボーダーの幅) * 本数 */
  text-align: center;
  margin-right: auto;
  margin-left: auto;
}

#root {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.title-container {
  border-radius: 4px 4px 0px 0px;
  box-shadow: 0px 4px 1px 1px #97a5a5, 0px 6px 1px 1px #3c4a4a, 0px 0px 1px 1px #3c4a4a;
}

.nav-title {
  margin: 0;
  padding: 8px;
  font-size: 1.1rem;
}

.dots {
  height: 9px;
  width: 9px;
  background: #0f1d1d;
}

.field {
  display: flex;
  flex-wrap: wrap;
  margin-top: 2px;
  color: white;
  background: #0f1d1d;
  border-radius: 0 0 4px 4px;
  border: 1px solid #ffffff30;
  box-shadow: 0px 4px 1px 1px #0c1818, 0px 6px 1px 1px #060f0d;
}

.snake {
  background: white;
}

.food {
  background: pink;
}

.navigation {
  display: flex;
  margin-top: 4px;
  justify-content: space-evenly;
  color: white;
  font-weight: bold;
  background: #020705;
  box-shadow: 0px 4px 1px 1px #020604, 0px 6px 1px 1px #010403;
  border: 1px solid #ffffff30;
}

.navigation-item {
  padding: 16px;
  display: flex;
  align-items: center;
}

.navigation-item-number-container {
  display: flex;
  padding: 0 8px;
  align-items: center;
  margin-left: 8px;
}

.num-board {
  display: flex;
  justify-content: center;
  align-items: center;
  background: #0f1d1d;
  height: 48px;
  width: 24px;
}

.difficulty-number {
  margin-right: 8px;
}

.difficulty-button {
  cursor: pointer;
  width: 8px;
  height: 8px;
  border: 8px solid #0f1d1d;
  margin: 8px;
}

.difficulty-button-container {
  background: #0f1d1d;
}

.difficulty-up {
  margin-top: 4px;
  border-bottom: 8px solid #f0f0f050;
  transition: all ease 0.5s;
}
.difficulty-up:hover {
  transform: scale(1.1);
  border-bottom: 8px solid white;
}
.difficulty-up:active {
  transform: scale(0.8);
  opacity: 0.8;
}

.difficulty-down {
  margin-bottom: 4px;
  border-top: 8px solid #f0f0f050;
  transition: all ease 0.5s;
}
.difficulty-down:hover {
  transform: scale(1.1);
  border-top: 8px solid white;
}
.difficulty-down:active {
  transform: scale(0.8);
  opacity: 0.8;
}

.btn {
  border: 0;
  cursor: pointer;
  color: white;
  border-radius: 4px;
  width: 100%;
  font-weight: bold;
  padding: 8px 16px;
  font-size: 1rem;
  text-transform: uppercase;
  transition: all ease 0.2s;
}

.btn-init,
.btn-suspended {
  box-shadow: 0px 4px 1px 1px #199a4b, 0px 6px 1px 1px #0b5420;
  background: #23ce6b;
}

.btn-playing {
  box-shadow: 0px 4px 1px 1px #d4a752, 0px 6px 1px 1px #956933;
  background: #ffd166;
}

.btn-gameover {
  box-shadow: 0px 4px 1px 1px #ce3959, 0px 6px 1px 1px #8b1c2c;
  background: #ef476f;
}

.btn:focus {
  outline: 0;
}

.btn:active {
  box-shadow: 0px 0px 0px 0px;
  transform: translateY(6px);
}

.btn-init:active,
.btn-suspended:active {
  background: #20bd60;
}

.btn-playing:active {
  background: #eabc5c;
}

.btn-gameover:active {
  background: #de4064;
}

.manipulation-panel {
  margin-top: 2px;
  display: flex;
  justify-content: center;
}

.manipulation-btn {
  min-width: 106px;
  margin: 4px 2px;
  background: #52b4e8;
  box-shadow: 0px 4px 1px 1px #4293c4, 0px 6px 1px 1px #21507b;
}

.btn-move:active {
  background: #4293c4;
}

.btn-left,
.btn-right {
  min-width: 106px;
  width: 106px;
}
.btn-left {
  margin-left: 0;
}
.btn-right {
  margin-right: 0;
}

.btn-up,
.btn-down {
  margin-left: 0px;
  margin-right: 0px;
}
</style>
