<template>
  <div>
    <transition>
      <div v-show="show" class="modal-bg">
        <div ref="modal" class="modal-container">
          <div class="modal-top">
            <h2>Rules</h2>
            <div class="modal_close--top">
              <img
                @click="handleClose"
                src="../assets/images/icon-close.svg"
                alt=""
              />
            </div>
          </div>
          <div class="modal-content">
            <img src="../assets/images/image-rules-bonus.svg" alt="" />
          </div>
          <div class="modal-bottom modal_close--bottom">
            <img
              @click="handleClose"
              src="../assets/images/icon-close.svg"
              alt=""
            />
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  const emit = defineEmits(['close'])
  const show = ref(false)
  const handleClose = () => {
    show.value = false
    setTimeout(() => {
      emit('close')
    }, 250)
  }
  const modal = ref(null)

  onMounted(() => {
    show.value = true
    if (modal.value) {
      document.addEventListener('click', (e) => {
        if (e.target !== modal.value && e.target.parenNode !== modal.value) {
          handleClose()
        }
      })
    }
  })

  // document.getElementById('pol');
  //       if(event.target != pol && event.target.parentNode != pol){
  //           pol.style.display = 'none';
  //       }
  // });
</script>

<style lang="scss" scoped>
  .modal-bg {
    position: fixed;
    inset: 0;
    display: block;
    background: rgba($color: #000000, $alpha: 0.4);
    z-index: 999;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .modal-container {
    background-color: #fff;
    width: 100%;
    height: 100vh;
    border-radius: 1rem;
    padding: 3rem;
    display: flex;
    flex-direction: column;
    transition: all 250ms ease-in-out;
    @media (min-width: 376px) {
      max-width: 400px;
      max-height: 460px;
    }
  }

  .modal-top {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 2rem;
    h2 {
      flex: 1;
      font-size: 3rem;
      font-weight: 600;
      text-transform: uppercase;
      margin: 0;
      text-align: center;
    }
    @media (min-width: 375px) {
      h2 {
        text-align: left;
      }
    }
  }
  .modal_close {
    &--top {
      display: none;
    }

    @media (min-width: 375px) {
      &--top {
        display: block;
      }
      &--bottom {
        display: none;
      }
    }
  }
  .modal-content {
    flex: 1 0 auto;
    max-height: 80%;
    display: flex;
    align-items: center;
    padding-bottom: 1rem;
    margin-bottom: 1rem;
    @media (min-width: 375px) {
      margin: 20px 0 0 0;
    }
    img {
      max-width: 100%;
    }
  }

  // Animations

  .v-enter-active,
  .v-leave-active {
    transition: all 250ms ease-in-out;
  }

  .v-enter-from {
    opacity: 0;
    .modal-container {
      transform: translateY(20px);
    }
  }
  .v-leave-to {
    opacity: 0;
    .modal-container {
      transform: translateY(20px);
    }
  }
</style>
