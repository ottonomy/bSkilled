<template>
  <div class="row-page">
    <v-introduction :introductionContent="introductionContent" :bgimage="bgimage"></v-introduction>
    <section class="section-right section-right-bg">
      <div class="section-right_container section-right_container-center">
        <v-indicator :pageVisited="pageVisited"></v-indicator>
        <div class="container">
          <Badge :badge-class="share.implication.badgeTemplate"/>
          <h1 class="create-badge-title">{{$t("ShareBadgeLinkPage.createdTitle")}}</h1>
          <p class="link-description">{{$t("ShareBadgeLinkPage.createdDescription")}}</p>
          <div class="container-link">
            <p class="link"> {{location + "sign/" + share.sid}}</p>
            <button class="button-copy" type="button" name="button"
            v-clipboard:copy="thingToCopy"
            v-clipboard:success="onCopy"
            v-clipboard:error="onError"
            >{{$t("ShareBadgeLinkPage.copy")}}</button>
          </div>
          <svg class="checkmark" v-if="copySucceeded === true" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 52 52"><circle class="checkmark__circle" cx="26" cy="26" r="25" fill="none"/><path class="checkmark__check" fill="none" d="M14.1 27.2l7.1 7.2 16.7-16.8"/></svg>
          <p class="copy-status" v-if="copySucceeded === true">{{$t("ShareBadgeLinkPage.copied")}}</p>
          <p class="copy-status" v-if="copySucceeded === false">{{$t("ShareBadgeLinkPage.controlCopy")}}</p>
          <v-button :onClick="handlerNewBadge">{{$t("ShareBadgeLinkPage.newbadge")}}</v-button>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { mapState } from "vuex";

import Introduction from "Components/IntroductionOfPage";
import Indicator from "Components/StepIndicator";
import Button from "Components/Button";
import Badge from "Components/BadgeToShare";

export default {
  components: {
    "v-introduction": Introduction,
    "v-indicator": Indicator,
    "v-button": Button,
    Badge
  },
  data() {
    return {
      flowStep: "share",
      location: process.env.LOCATION,
      copySucceeded: null,
      selectedBoolean: true,
      pageVisited: 2,
      bgimage: {
        img: "./share_step3.png",
        position: "307px",
        size: "35%",
        left: "15px"
      }
    };
  },
  methods: {
    onCopy: function(e) {
      this.copySucceeded = true;
    },
    onError: function(e) {
      this.copySucceeded = false;
    },
    handlerNewBadge() {
      this.$router.push({ name: "search" });
    }
  },
  computed: {
    introductionContent() {
      return {
        title: this.$t("ShareBadgeLinkPage.introductionTitle"),
        text: this.$t("ShareBadgeLinkPage.introductionDescription")
      };
    },
    ...mapState(["share", "badgeTemplate", "recipient"]),
    thingToCopy() {
      return process.env.LOCATION + "#/share/" + this.share.sid;
    }
  },
  activated() {
    this.$store.commit("SET_CURRENT_FLOW_STEP", this.flowStep);
  }
};
</script>

<style lang="scss" scoped>
@import "~Vars";

.container-link {
  display: flex;
  margin-bottom: 30px;
}

.link {
  display: flex;
  align-items: center;
  background: #fff;
  border: 0;
  border-radius: 5px 0 0 5px;
  box-shadow: 0 0 8px 0 rgba($darkblue, 0.25);
  color: rgba($darkblue, 0.5);
  font-weight: 400;
  font-size: 13px;
  height: 40px;
  padding-left: 15px;
  width: 290px;
  text-decoration: underline;
}

.link-description {
  text-align: center;
  width: 321px;
  line-height: 1.5;
  margin-bottom: 30px;
  font-size: 15px;
  color: $darkblue;
}

.button-copy {
  cursor: pointer;
  font-size: 13px;
  border: none;
  background-color: $darkblue;
  color: white;
  border-radius: 0 5px 5px 0;
  width: 110px;
}

.copy-status {
  font-size: 13px;
  margin-top: 7px;
  margin-bottom: 20px;
}

.checkmark__circle {
  stroke-dasharray: 166;
  stroke-dashoffset: 166;
  stroke-width: 2;
  stroke-miterlimit: 10;
  stroke: $green;
  fill: none;
  animation: stroke 0.6s cubic-bezier(0.65, 0, 0.45, 1) forwards;
}

.checkmark {
  margin-top: -7px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: block;
  stroke-width: 2;
  stroke: #fff;
  stroke-miterlimit: 10;
  box-shadow: inset 0px 0px 0px $green;
  animation: fill 0.4s ease-in-out 0.4s forwards,
    scale 0.3s ease-in-out 0.9s both;
}

.checkmark__check {
  transform-origin: 50% 50%;
  stroke-dasharray: 48;
  stroke-dashoffset: 48;
  animation: stroke 0.3s cubic-bezier(0.65, 0, 0.45, 1) 0.8s forwards;
}

@keyframes stroke {
  100% {
    stroke-dashoffset: 0;
  }
}
@keyframes scale {
  0%,
  100% {
    transform: none;
  }
  50% {
    transform: scale3d(1.1, 1.1, 1);
  }
}
@keyframes fill {
  100% {
    box-shadow: inset 0px 0px 0px 30px $green;
  }
}

.badge {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.badge-icon {
  position: absolute;
  filter: url("#linear");
  opacity: 0.6;
}

.created-badge-name {
  font-weight: 700;
  margin-top: 10px;
  margin-bottom: 25px;
  text-transform: lowercase;
  color: $darkblue;
}

.create-badge-title {
  color: $darkblue;
}
</style>
