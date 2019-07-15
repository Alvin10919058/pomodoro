<template>
  <div>
    <div class="left-section">
      <div class="container">
        <div class="add-to-do-item">
          <input type="text" v-model="inputText" placeholder="ADD A NEW MISSION..." />
          <i @click="addItem" class="add-item-icon material-icons">
            add
          </i>
        </div>
        <div v-for="item in list" :key="item.id" v-if="item.isActive&&!item.isDone" class="now-to-do-item">
          <div>
            <input :id="item.id" v-model="item.isDone" type="checkbox" />
            <label :for="item.id">
              <span class="check">✓</span>
            </label>
          </div>
          <div class="now-task">
            <div class="title">{{item.title}}</div>
          </div>
        </div>
        <div class="time-counter">{{prettyTime}}</div>
        <div class="to-do-list-section">
          <div class="task-list">
            <div v-for="item in list" :key="item.id" v-if="!item.isActive&&!item.isDone" class="to-do-item">
              <div>
                <input :id="item.id" v-model="item.isDone" type="checkbox" />
                <label :for="item.id">
                  <span class="check">✓</span>
                </label>
              </div>
              <div class="now-task">
                <div class="title">{{item.title}}</div>
              </div>
              <div class="play-icon">
                 <i @click="active(item.id)" class="material-icons">play_circle_outline</i>
              </div>
            </div>
          </div>
          <div class="more-section">
            <span class="more-link">more</span>
          </div>
        </div>
      </div>
    </div>
    <div class="counter-section">
      <div class="circle" :class="{'bg-white':isRunning}">
        <svg class="circle-svg" id="circleSvg" xmlns="http://www.w3.org/2000/svg">
          <circle cx="50%" cy="50%" r="261" stroke-width="21" stroke="#FF4384" :stroke-dashoffset="time/1500*360+'%'"></circle>
        </svg>
        <div class="play-btn" :class="{'color-pink':isRunning}">
          <i v-if="!isRunning" @click="start" class="play-icon material-icons">play_circle_filled</i>
          <i v-if="isRunning" @click="stop" class="pause-icon material-icons">pause_circle_filled</i>
          <div class="dot" :class="{'bg-pink':isRunning}"></div>
        </div>
      </div>
    </div>
    <div class="menu-bar">
      <div class="function-list">
        <i class="material-icons">list</i>
        <i class="material-icons">assessment</i>
        <i class="material-icons">library_music</i>
      </div>
      <div class="menu-title">POMODORO</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isRunning: false,
      timer: null,
      time: 1500,
      minutes: 0,
      secondes: 0,
      inputText: "",
      list: [
        {
          id: 1,
          title: "the first thing to do",
          isDone: false,
          isActive: true
        },
        {
          id: 2,
          title: "the second thing to do",
          isDone: false,
          isActive: false
        },
        {
          id: 3,
          title: "the third thing to do",
          isDone: false,
          isActive: false
        }
      ]
      // workTime: 1500,
      // breakTime: 300
    };
  },
  computed: {
    prettyTime() {
      let time = this.time / 60;
      let minutes = parseInt(time);
      var secondes = Math.round((time - minutes) * 60);
      if (secondes / 10 < 1) {
        secondes = "0" + secondes;
      }
      return minutes + ":" + secondes;
    }
  },
  methods: {
    start() {
      this.isRunning = true;
      if (!this.timer) {
        this.timer = setInterval(() => {
          if (this.time > 0) {
            this.time--;
          } else {
            clearInterval(this.timer);
            //this.sound.play();
            this.reset();
          }
        }, 1000);
      }
    },
    stop() {
      this.isRunning = false;
      clearInterval(this.timer);
      this.timer = null;
    },
    reset() {
      this.stop();
      this.time = 1500;
    },
    active(targetId) {
      this.list.forEach(item => {
        if (item.id == targetId) {
          item.isActive = true;
        } else {
          item.isActive = false;
        }
      });
    },
    addItem() {
      this.list.push({
        id: this.list.length + 1,
        title: this.inputText,
        isDone: false,
        isActive: false
      });
      this.inputText = "";
    }
  }
};
</script>


<style lang="scss" scoped>
.left-section {
  box-sizing: border-box;
  padding: 48px 75px;
  background-color: #ffedf7;
  //   display: inline-block;
  width: 65%;
  height: 100vh;
  .container {
    width: 470px;
    .add-to-do-item {
      margin-bottom: 110px;
      // background-color: #fff;
      width: 32vw;
      // height: 28px;
      // margin: auto;
      display: flex !important;
      input {
        font-weight: 600;
        font-family: inherit;
        font-size: 16px;
        color: #ff4384 !important;
        padding: 1rem !important;
        width: 100% !important;
        border: 0 !important;
        outline: none;
      }
      input::placeholder {
        font-family: inherit;
        font-style: italic;
        font-weight: bold;
        font-size: 16px;
        color: #ff4384 !important;
      }
      .add-item-icon {
        cursor: pointer;
        color: #ff4384 !important;
        padding: 13px 15px 0px 0px;
        background-color: #fff;
        font-size: 25px;
      }
    }
    .now-to-do-item {
      display: flex;
      input {
        display: none;
      }
      input:checked ~ label .check {
        opacity: 1;
      }
      label {
        position: relative;
        width: 48px;
        height: 48px;
        border-color: rgb(0, 49, 100);
        display: block;
        border: 2px solid #003164;
        border-radius: 50%;
        cursor: pointer;
        margin-right: 1rem;
        .check {
          font-size: 32px;
          color: rgb(0, 49, 100);
          position: absolute;
          opacity: 0;
          -webkit-box-sizing: border-box;
          box-sizing: border-box;
          background: transparent;
          color: #003164;
          line-height: 1;
          top: 50%;
          left: 50%;
          -webkit-transform: translate(-50%, -50%);
          transform: translate(-50%, -50%);
          -webkit-transition: border-color 0.2s ease-in-out,
            opacity 0.2s ease-in-out;
          transition: border-color 0.2s ease-in-out, opacity 0.2;
        }
      }
      .now-task {
        font-family: Roboto, Microsoft JhengHei, sans-serif;
        display: flex;
        align-items: center;
        .title {
          font-size: 1.5rem;
          font-weight: bold;
        }
      }
    }
    .time-counter {
      font-size: 11rem;
      font-weight: 700;
      color: #ff4384;
    }
    .to-do-list-section {
      .task-list {
        max-height: 137px;
        overflow: hidden;
        .to-do-item {
          border-bottom: 2px solid rgba(0, 49, 100, 0.2);
          margin-bottom: 0.5rem;
          padding: 5px 0px;
          display: flex;
          input {
            display: none;
          }
          input:checked ~ label .check {
            opacity: 1;
          }
          label {
            position: relative;
            width: 24px;
            height: 24px;
            border-color: rgb(0, 49, 100);
            display: block;
            border: 2px solid #003164;
            border-radius: 50%;
            cursor: pointer;
            margin-right: 1rem;
            .check {
              font-size: 16px;
              color: rgb(0, 49, 100);
              position: absolute;
              opacity: 0;
              -webkit-box-sizing: border-box;
              box-sizing: border-box;
              background: transparent;
              color: #003164;
              line-height: 1;
              top: 50%;
              left: 50%;
              -webkit-transform: translate(-50%, -50%);
              transform: translate(-50%, -50%);
              -webkit-transition: border-color 0.2s ease-in-out,
                opacity 0.2s ease-in-out;
              transition: border-color 0.2s ease-in-out, opacity 0.2;
            }
          }
          .now-task {
            font-family: Roboto, Microsoft JhengHei, sans-serif;
            display: flex;
            align-items: center;
            .title {
              color: #003164;
              font-size: 1rem;
              font-weight: bold;
            }
          }
          .play-icon {
            margin-left: auto;
            cursor: pointer;
            display: flex;
            align-items: center;
            i {
              color: #003164;
            }
          }
        }
      }
      .more-section {
        color: #ff4384;
        font-size: 1rem;
        text-align: right;
        margin-top: 8px;
        .more-link {
          font-weight: bold;
          cursor: pointer;
          font-family: Roboto, Microsoft JhengHei, sans-serif;
          text-transform: uppercase;
        }
      }
    }
  }
}
.bg-pink {
  background-color: #ff4384 !important;
}
.color-pink {
  color: #ff4384 !important;
}
.bg-white {
  background-color: #fff !important;
}
.color-white {
  color: #fff !important;
}
.counter-section {
  width: 540px;
  height: 540px;
  border: 4px solid #ff4384;
  border-radius: 50%;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
  position: absolute;
  left: 65%;
  top: 50%;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  .circle {
    border: 4px solid #ff4384;
    display: flex;
    justify-content: center;
    align-items: center;
    -webkit-transition: 0.3s;
    transition: 0.3s;
    width: 500px;
    height: 500px;
    border-radius: 50%;
    background-color: #ff4384;
    .circle-svg {
      position: absolute;
      top: 50%;
      left: 50%;
      -webkit-transform: translate(-50%, -50%) rotate(-90deg);
      transform: translate(-50%, -50%) rotate(-90deg);
      width: 540px;
      height: 540px;
      stroke-dasharray: 360%;
      stroke-dashoffset: 360%;
      fill: none;
    }
    .play-btn {
      z-index: 0;
      color: #fff;
      width: 6rem;
      height: 6rem;
      position: relative;
      .play-icon {
        border-radius: 50%;
        font-size: 6.2rem;
        cursor: pointer;
        -webkit-transition: 0.3s;
        transition: 0.3s;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
      }
      .play-icon:hover {
        transform: scale(1.2);
      }
      .pause-icon {
        border-radius: 50%;
        background-color: white;
        font-size: 6.2rem;
        cursor: pointer;
        -webkit-transition: 0.3s;
        transition: 0.3s;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
      }
      .pause-icon:hover {
        transform: scale(1.2);
      }
      .dot {
        width: 14px;
        height: 14px;
        background-color: #fff;
        cursor: pointer;
        position: absolute;
        bottom: 0;
        right: 0;
        -webkit-transform: translateX(100%);
        transform: translateX(100%);
        -webkit-transition: 0.3s;
        transition: 0.3s;
      }
      .dot:hover {
        transform: scale(1.3) translateX(100%);
      }
    }
  }
}
.menu-bar {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  // padding: 48px 0;
  height: 100%;
  position: absolute;
  top: 0px;
  right: 60px;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: space-between;
  .function-list {
    padding-top: 48px;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-direction: column;
    flex-direction: column;
    i {
      font-size: 36px;
      width: 36px;
      height: 36px;
      color: #ffedf7;
      cursor: pointer;
      -webkit-transition: 0.3s;
      transition: 0.3s;
      margin-bottom: 48px;
    }
  }
  .menu-title {
    user-select: none;
    padding-bottom: 48px;
    font-size: 1.5rem;
    font-weight: 700;
    color: #ffedf7;
    -webkit-writing-mode: vertical-lr;
    -ms-writing-mode: tb-lr;
    writing-mode: vertical-lr;
  }
}
</style>
