<template>
  <div class="loading">
    <!-- <span></span> -->
    <div class="loader">
      <div class="line"></div>
      <div class="line"></div>
      <div class="line"></div>
      <div class="line"></div>
      <div class="line"></div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
$loader-clr: #ff9500;

.loading {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;

  .loader {
    width: 18rem;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    .line {
      background: $loader-clr;
      width: 1rem;
      height: 4rem;
      border-radius: 1rem;
      &:nth-child(1) {
        animation: animationLoader2 0.5s 0.6s infinite;
      }
      &:nth-child(2) {
        animation: animationLoader2 0.5s 0.3s infinite;
      }
      &:nth-child(3) {
        animation: animationLoader2 0.5s infinite;
      }
      &:nth-child(4) {
        animation: animationLoader2 0.5s 0.3s infinite;
      }
      &:nth-child(5) {
        animation: animationLoader2 0.5s 0.6s infinite;
      }
      @keyframes animationLoader2 {
        0% {
          height: 5rem;
        }
        50% {
          height: 15rem;
        }
        100% {
          height: 5rem;
        }
      }
    }
  }
}

@media screen and (max-width: 991px) {
  .loading {
    gap: 2rem;
  }
}
</style>