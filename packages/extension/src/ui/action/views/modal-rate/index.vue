<template>
  <div class="rate__container">
    <div class="rate__overlay" @click="close()" />
    <div class="rate__wrap">
      <div class="rate__header">
        <h2>Enjoying Self Wallet so far?</h2>
        <a class="rate__close" @click="close()">
          <close-icon />
        </a>
      </div>
      <p>Let us know how you feel, your feedback is important to us.</p>
      <base-button title="Sure, I'll rate it" :click="goToRate" />
      <div class="rate__button-indent"></div>
      <base-button
        title="I have a feedback"
        :no-background="true"
        :click="goToFeedback"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import CloseIcon from "@action/icons/common/close-icon.vue";
import BaseButton from "@action/components/base-button/index.vue";
import RateState from "@/libs/rate-state";
import { detectBrowser, BROWSER_NAMES, openLink } from "@action/utils/browser";

const rateState = new RateState();

const emit = defineEmits<{
  (e: "close:popup"): void;
}>();
const close = async () => {
  await rateState.resetPopupTimer();
  emit("close:popup");
};

const goToFeedback = async () => {
  await rateState.resetPopupTimer();

  openLink("mailto:info@selfcrypto.io");
};

const rateLinks: Record<string, string> = {
  [BROWSER_NAMES.chrome]: "mailto:info@selfcrypto.io",
  [BROWSER_NAMES.firefox]: "mailto:info@selfcrypto.io",
  [BROWSER_NAMES.opera]: "mailto:info@selfcrypto.io",
  [BROWSER_NAMES.edge]: "mailto:info@selfcrypto.io",
  [BROWSER_NAMES.brave]: "mailto:info@selfcrypto.io",
  [BROWSER_NAMES.safari]: "mailto:info@selfcrypto.io",
};

const goToRate = async () => {
  await rateState.setRated();

  const browser = detectBrowser();

  const rateLink = rateLinks[browser];

  if (rateLink) {
    openLink(rateLink);
  } else {
    openLink(rateLinks[BROWSER_NAMES.chrome]);
  }
};
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.rate {
  width: 100%;
  height: auto;
  box-sizing: border-box;

  &__wrap {
    background: @white;
    box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.039),
      0px 7px 24px rgba(0, 0, 0, 0.19);
    border-radius: 12px;
    box-sizing: border-box;
    width: 360px;
    height: auto;
    z-index: 107;
    position: relative;
    overflow-x: hidden;
    padding: 24px 16px;

    p {
      font-style: normal;
      font-weight: 400;
      font-size: 16px;
      line-height: 24px;
      color: @secondaryLabel;
      margin: 0 0 12px 0;
    }
  }

  &__header {
    width: 100%;
    background: @white;
    box-sizing: border-box;
    padding: 0 40px 12px 0;

    h2 {
      font-style: normal;
      font-weight: bold;
      font-size: 24px;
      line-height: 32px;
      margin: 0;
      color: @primaryLabel;
    }
  }

  &__close {
    position: absolute;
    top: 8px;
    right: 8px;
    border-radius: 8px;
    cursor: pointer;
    transition: background 300ms ease-in-out;
    font-size: 0;

    &:hover {
      background: @black007;
    }
  }

  &__container {
    width: 800px;
    height: 100%;
    left: 0px;
    top: 0px;
    position: fixed;
    z-index: 105;
    display: flex;
    box-sizing: border-box;
    justify-content: center;
    align-items: center;
    flex-direction: row;
  }

  &__overlay {
    background: rgba(0, 0, 0, 0.32);
    width: 100%;
    height: 100%;
    left: 0px;
    top: 0px;
    position: absolute;
    z-index: 106;
  }

  &__block {
    padding: 12px 0;

    &:nth-child(2) {
      padding-top: 0;
    }
  }
  &__button-indent {
    margin-bottom: 8px;
  }
}
</style>
