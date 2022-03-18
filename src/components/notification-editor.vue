<template>
  <modal-wrapper @closeModal="takeModalState('close')">

    <div class="modal__top">
      <h4 class="modal__title title">Редактор типов уведомлений</h4>

      <div class="body__header">

        <div class="input-wrapper">
          <label class="set-name__label header__text" for="set-name">Название типа уведомлений</label>
          <input id="set-name" type="text" :placeholder="placeholderText" v-model="itemName">
        </div>

        <drop-menu blockClass="mark-wrapper" :itemSelected="itemSelected">

          <template v-slot:name>
            <p class="settings__name">
              Метка\Номер
            </p>
          </template>

          <template v-slot:text>
            <p class="settings__text" v-if="currentMark === 0">
              <star-icon class="modal__star" />
            </p>
            <p class="settings__text" v-else-if="currentMark === 1">
              <emoji-normal-icon />
            </p>
            <p class="settings__text" v-else-if="currentMark === 2">
              <trend-up-icon />
            </p>
            <p class="settings__text" v-else-if="currentMark === 3">
              <cake-icon />
            </p>
            <p class="settings__text" v-else>
              <sms-tracking-icon />
            </p>
          </template>

          <template v-slot:item>

            <li>
              <button class="drop__item" @click="selectItem(0, 'mark')">
                <star-icon />
              </button>
            </li>

            <li>
              <button class="drop__item" @click="selectItem(1, 'mark')">
                <emoji-normal-icon />
              </button>
            </li>

            <li>
              <button class="drop__item" @click="selectItem(2, 'mark')">
                <trend-up-icon />
              </button>
            </li>

            <li>
              <button class="drop__item" @click="selectItem(3, 'mark')">
                <cake-icon />
              </button>
            </li>

            <li>
              <button class="drop__item" @click="selectItem(4, 'mark')">
                <sms-tracking-icon />
              </button>
            </li>

          </template>

        </drop-menu>

      </div>
    </div>

    <div class="segment-settings">

      <drop-menu :itemSelected="itemSelected">

        <template v-slot:name>
          <p class="settings__name">
            Группа сегментов
          </p>
        </template>

        <template v-slot:text>
          <p class="settings__text">
            {{ groupList[currentGroup] }}
          </p>
        </template>

        <template v-slot:item>

          <li v-for="(item, index) in groupList" :key="index">
            <button class="drop__item" @click="selectItem(index, 'group')">{{ item }}</button>
          </li>

        </template>

      </drop-menu>

      <drop-menu :itemSelected="itemSelected">

        <template v-slot:name>
          <p class="settings__name">
            Сегмент
          </p>
        </template>

        <template v-slot:text>
          <div class="flex">
            <span class="item-number settings__number">0{{ currentSegment + 1 }}</span>
            <p class="settings__text">{{ segmentList[currentSegment] }}</p>
          </div>
        </template>

        <template v-slot:item>

          <li v-for="(item, index) in segmentList" :key="index">
            <button class="drop__item" @click="selectItem(index, 'segment')">
              <span class="item-number settings__number">0{{ index + 1 }}</span>
              <p class="settings__text">{{ item }}</p>
            </button>
          </li>

        </template>

      </drop-menu>

    </div>

    <modal-main />

    <modal-buttons buttonText="Сохранить изменения" @toggleModal="takeModalState($event)"></modal-buttons>

  </modal-wrapper>
</template>

<script>
import { ref, computed } from "vue";
import modalWrapper from "@/components/modal-wrapper";
import modalMain from "@/components/modal-main";
import modalButtons from "@/components/modal-buttons";
import starIcon from "@/components/icon/star-icon";
import dropMenu from "@/components/drop-menu";
import emojiNormalIcon from "@/components/icon/emoji-normal-icon";
import cakeIcon from "@/components/icon/cake-icon";
import smsTrackingIcon from "@/components/icon/sms-tracking-icon";
import trendUpIcon from "@/components/icon/trend-up-icon";

export default {
  name: "notification-editor",
  components: {
    modalWrapper: modalWrapper,
    modalMain: modalMain,
    modalButtons: modalButtons,
    starIcon: starIcon,
    dropMenu: dropMenu,
    emojiNormalIcon: emojiNormalIcon,
    cakeIcon: cakeIcon,
    smsTrackingIcon: smsTrackingIcon,
    trendUpIcon: trendUpIcon
  },
  props: {
    currentItem: {
      type: Object,
      required: false
    }
  },
  emits: ['toggleModal'],
  setup(props, { emit }) {

    const placeholderText = computed(() => {
      if(props.currentItem) {
        return props.currentItem.text;
      }
      return '';
    })

    const groupList = [
        'Стандартные сегменты',
        'Нестандартные сегменты',
        'Другие сегменты'
    ];

    const segmentList = [
        'Чемпионы',
        'Лояльные, постоянные',
        'Растущие'
    ]

    const currentMark = ref(0);
    const currentGroup = ref(0);
    const currentSegment = ref(0);

    const itemSelected = ref(false);

    function selectItem(index, list) {

      if(list === 'group') {
        currentGroup.value = index;
      } else if(list === 'segment') {
        currentSegment.value = index;
      } else {
        currentMark.value = index;
      }

      itemSelected.value = true;
      setTimeout(() => {
        itemSelected.value = false;
      }, 500);

    }

    function takeModalState(type) {
      emptyName();

      if(type === 'submit') {
        emit('toggleModal', {
          modal: 'notificationEditor',
          updateName: itemName.value,
          state: false,
          type: type
        })
      } else {
        emit('toggleModal', {
          modal: 'notificationEditor',
          state: false
        })
      }

    }

    const itemName = ref('');
    function emptyName() {

      if(itemName.value === '' || itemName.value === ' ') {
        itemName.value = props.currentItem.text;
      }

    }


    return {
      takeModalState,
      groupList,
      currentGroup,
      selectItem,
      segmentList,
      currentSegment,
      currentMark,
      itemSelected,
      itemName,
      placeholderText
    }

  }
}
</script>

<style scoped>
.modal__top {
  padding-bottom: 45px;
  border-bottom: 1px solid #EFF0F0;
}

.modal__title {
  margin-bottom: 30px;
}

.body__header {
  display: flex;
  justify-content: space-between;
}

.set-name__label {
  font-size: 14px;
  color: #6D6D6D;
}

#set-name {
  width: 656px;
  height: 48px;

  padding: 12px 20px;
  border-radius: 12px;
  box-sizing: border-box;

  background-color: #F6F9FC;
}

.header__text {
  display: block;
  margin-bottom: 5px;
}

.mark__text {
  font-size: 14px;
  color: #6D6D6D;
}

.modal__star {
  stroke: #A1A2A2;
}

.mark__line {
  width: 1px;
  height: 24px;

  background-color: rgba(0, 0, 0, 0.1);
}

.segment-settings {
  display: flex;
  justify-content: space-between;
  margin-top: 35px;
}

.settings__name {
  margin-bottom: 5px;

  font-size: 14px;
  color: #6D6D6D;
}

.settings__block {
  display: flex;
  align-items: center;

  width: 377px;
  height: 48px;
  padding: 0 20px;
  border-radius: 12px;
  box-sizing: border-box;

  background-color: #F6F9FC;
}

.settings__number {
  width: 24px;
  height: 24px;
  margin-right: 10px;

  border-radius: 9px;

  font-size: 12px;
}

.settings__text {
  font-size: 16px;
}

.settings__line {
  width: 1px;
  height: 24px;
  margin-right: 15px;
  margin-right: 15px;

  background-color: rgba(0, 0, 0, 0.1);
}

.flex {
  display: flex;
  align-items: center;
}

.drop__item {
  display: flex;
  align-items: center;

  height: 30px;
  margin-bottom: 20px;

  font-size: 15px;
}

.drop__item--last {
  margin-bottom: 0;
}

</style>