<template>
  <div class="modal-main">
    <button
        :class="['main__section', 'main__section1', {'main__section--active': activeSection === 0}]"
        @click="selectSection(0)"
    >
      Push
    </button>
    <button
        :class="['main__section', 'main__section2', {'main__section--active': activeSection === 1}]"
        @click="selectSection(1)"
    >
      WhatsApp
    </button>
    <button
        :class="['main__section', 'main__section3', {'main__section--active': activeSection === 2}]"
        @click="selectSection(2)"
    >
      SMS
    </button>

    <div class="main__middle">


      <VueEmoji ref="emoji" @input="onInput" :value="myText" />


      <transition name="overlayAm">
        <spec-overlay v-show="overlayState === 1" @addVariable="addVariable($event)" />
      </transition>

      <transition name="overlayAm">
        <emoji-overlay v-show="overlayState === 2" />
      </transition>

      <transition name="overlayAm">
        <preview-overlay v-show="overlayState === 3" />
      </transition>

    </div>

    <div class="main__bottom">
      <div class="bottom__left">
        <button class="bottom__button bottom__button1" @click="toggleOverlay(1)">Переменные</button>
<!--        <button class="bottom__button bottom__button2" @click="toggleOverlay(2)">-->
<!--          <emoji-normal-icon />-->
<!--        </button>-->
      </div>

      <button class="bottom__button bottom__button4">Очистить</button>
    </div>
  </div>
  <close-icon v-show="false" />

</template>

<script>
import { ref } from "vue";
import specOverlay from "@/components/spec-overlay";
import emojiOverlay from "@/components/emoji-overlay";
import previewOverlay from "@/components/preview-overlay";
import closeIcon from "@/components/icon/close-icon";
import VueEmoji from 'emoji-vue'

export default {
  name: "modal-main",
  components: {
    specOverlay: specOverlay,
    emojiOverlay: emojiOverlay,
    previewOverlay: previewOverlay,
    closeIcon: closeIcon,
    VueEmoji: VueEmoji
  },
  setup() {
    const activeSection = ref(0);

    function selectSection(number) {
      activeSection.value = number;
    }

    const overlayState = ref(0);

    function toggleOverlay(index) {
      if(overlayState.value === index) {
        overlayState.value = 0;
      } else {
        overlayState.value = index;
      }
    }

    // 1-----------------------------------------------------------------------------------------------------

    const variableList = [
      {
        text: 'дата и время',
        color: 'blue',
        code : '%DATETIME%'
      },
      {
        text: 'часы и минуты',
        color: 'blue',
        code : '%HOURMINUTES%'
      },
      {
        text: 'дата событитя',
        color: 'blue',
        code : '%DATE%'
      },
      {
        text: 'дней до события',
        color: 'blue',
        code : '%DAYS%'
      },
      {
        text: 'имя клиента',
        color: 'red',
        code : '%CLIENT_NAME%'
      },
      {
        text: 'имя сотрудника',
        color: 'red',
        code : '%MASTER_NAME%'
      },
      {
        text: 'название должности',
        color: 'green',
        code : '%POSITION_TITLE%'
      },
      {
        text: 'название услуги',
        color: 'green',
        code : '%SERVICE_TITLE%'
      },{
        text: 'ссылка на карту лояльности',
        color: 'dark-blue',
        code : '%CARD_URL%'
      },
      {
        text: 'скидка в процентах',
        color: 'dark-blue',
        code : '%DISCOUNT%%'
      },
      {
        text: 'ссылка на виджет ycliens',
        color: 'purple',
        code : '%LINK%'
      },
      {
        text: 'номер телефона',
        color: 'purple',
        code : '%CONTACT_PHONE%'
      }
    ]

    function onInput(event) {

      if(event.target) {
        console.log(event)         //event.target.innerHTML
      }

    }

    const emoji = ref(null);
    const myText = ref('');

    function clearTextarea(){
      emoji.value.clear();
    }

    function addVariable(index) {

      let textStyle = 'padding: 5px 14px; border-radius: 49px; font-size: 14px; color: #fff;';

      if(variableList[index].color === 'blue') {
        textStyle += 'background-color: #5CC3C6;';
      } else if(variableList[index].color === 'red') {
        textStyle += 'background-color: #DD6068;';
      } else if(variableList[index].color === 'green') {
        textStyle += 'background-color: #8ECD88;';
      } else if(variableList[index].color === 'dark-blue') {
        textStyle += 'background-color: #328FE4;';
      } else if(variableList[index].color === 'purple') {
        textStyle += 'background-color: #9D58A3;';
      }

      myText.value = '<span ' +  `style="${textStyle}"` + '>' + variableList[index].text + '</span>' + '&nbsp;';

    }

    // 2-----------------------------------------------------------------------------------------------------


    return {
      activeSection,
      selectSection,
      overlayState,
      toggleOverlay,
      variableList,
      emoji,
      clearTextarea,
      myText,
      addVariable,
      onInput
    }
  }
}
</script>

<style scoped>

.close-icon {
  width: 8px;
  height: 8px;
  margin-left: 10px;

  background-image: url("../assets/img/close-square.png");
}

.modal-main {
  position: relative;
  margin-top: 95px;
}

.main__middle {
  position: relative;

  height: 280px;
  padding: 40px;
  box-sizing: border-box;

  border-radius: 0 12px 0 0;

  background: #F6F9FC;
}

.main__section {
  position: absolute;
  top: -42px;

  display: flex;
  align-items: center;
  height: 42px;

  padding: 12px 24px;
  border-radius: 12px 12px 0 0;
  box-sizing: border-box;
  transition: .2s all ease;

  font-size: 14px;

  background: #F0F3F8;
}

.main__section--active {
  top: -49px;
  height: 49px;
  background-color: #F6F9FC;
}

.main__section1 {
  left: 0;
}

.main__section2 {
  left: 86px;
}

.main__section3 {
  left: 204px;
}

.main__bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;

  height: 75px;
  border-top: 1px solid #E1E2E2;
  border-radius: 0 0 12px 12px;

  background: #F0F3F8;
}

.bottom__left {
  display: flex;
  margin-left: 15px;
}

.bottom__button {
  height: 44px;
  padding: 10px 24px;

  box-shadow: 0 1px 2px rgba(21, 21, 21, 0.15);
  border-radius: 12px;
  box-sizing: border-box;

  font-size: 14px;
  color: #6D6D6D;

  background: #FFFFFF;
}

.bottom__button1 {
  margin-right: 15px;
}

.bottom__button2 {
  margin-right: 15px;
}

.bottom__button4 {
  height: 44px;
  margin-right: 15px;
  box-sizing: border-box;
}

.text--color {
  padding: 5px 14px;
  border-radius: 49px;

  font-size: 14px;
  color: #fff;
}

.text--blue {
  background: #5CC3C6;
}

.text--red {
  background-color: #DD6068;
}

.text--green {
  background-color: #8ECD88;
}

.text--dark-blue {
  background-color: #328FE4;
}

.text--purple {
  background-color: #9D58A3;
}

.overlayAm-enter-active,
.overlayAm-leave-active {
  transition: 0.3s all ease;
}

.overlayAm-enter-from,
.overlayAm-leave-to {
  bottom: -30px;
  opacity: 0;
}

</style>