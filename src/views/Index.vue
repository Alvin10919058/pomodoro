<template>
  <div>
    <div class="left-section">
      <div class="container">
        <div class="add-to-do-item">
          <input type="text" placeholder="ADD A NEW MISSION..." />
          <i class="add-item-icon fas fa-plus"></i>
        </div>
        <div class="now-to-do-item">
          <input type="radio" />
          <p>the first thing today</p>
        </div>
        <div class="time-counter">25:00</div>
        <div class="to-do-list-section">
          <ul>
            <li>
              <input type="radio" />
              <p>the second thing today</p>
            </li>
            <li>
              <input type="radio" />
              <p>the third thing today</p>
            </li>
            <li>
              <input type="radio" />
              <p>the forth thing today</p>
            </li>
          </ul>
          <span>more</span>
        </div>
      </div>
    </div>
    <!-- <div class="right-section">2312312</div> -->
  </div>
</template>

<script>
export default {
  data() {
    return {};
  }
};
</script>


<style lang="scss" scoped>
.left-section {
  box-sizing: border-box;
  padding: 48px 100px;
  background-color: #ffedf7;
  //   display: inline-block;
  width: 65%;
  height: 100vh;
  .container {
    //margin: 0px 100px 0px 100px;
    .add-to-do-item {
      // background-color: #fff;
      width: 32vw;
      // height: 28px;
      // margin: auto;
      display: flex !important;
      input {
        font-size: 16px;
        color: #ff4384 !important;
        padding: 1rem !important;
        width: 100% !important;
        border: 0 !important;
        outline: none;
      }
      input::placeholder {
        font-size: 16px;
        font-weight: 600;
        color: #ff4384 !important;
      }
      .add-item-icon {
          cursor: pointer;
        color: #ff4384 !important;
        padding: 15px 15px 0px 0px;
        background-color: #fff;
        font-size: 20px;
      }
    }
  }
}
// .right-section {
//   display: inline-block;
//   width: 35%;
//   height: 100vh;
//   background-color: #003164;
// }
</style>
