<template>
  <b-container fluid>
    <div class="clearfix py-1">
      <div class="float-left">
        <router-link :to="{ name: 'searchPage' }"
          ><span class="font-default">&larr;</span> Return to search</router-link
        >
      </div>
      <div class="float-right">
        <a v-if="showSideMenus" href="#" @click.prevent="addCard(card)" :aria-label="addCardText"
          ><font-awesome-icon icon="plus-square" /> {{ addCardText }}</a
        >
      </div>
    </div>
    <template v-if="cardIndex">
      <h3 class="my-2">{{ cardData.name }}</h3>
      <b-row>
        <!-- Image -->
        <b-col cols="12" md="6" class="d-flex justify-content-center mb-2">
          <img v-if="imageUrl" :src="imageUrlOverride || imageUrl" class="card-image" />
        </b-col>

        <b-col cols="12" md="6">
          <div class="p-1">
            <!-- Name -->
            <div class="clearfix">
              <div class="card-stat-label"><span>Card Name:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.name }}</span>
              </div>
            </div>
            <!-- Level -->
            <div v-if="cardData.level" class="clearfix">
              <div class="card-stat-label"><span>Level:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.level }}</span>
              </div>
            </div>
            <!-- Alignment -->
            <div v-if="cardData.alignment" class="clearfix">
              <div class="card-stat-label"><span>Alignment:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.alignment }}</span>
              </div>
            </div>
            <!-- Type -->
            <div v-if="cardData.type" class="clearfix">
              <div class="card-stat-label"><span>Type:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.type }}</span>
              </div>
            </div>
            <!-- Class -->
            <div v-if="cardData.class" class="clearfix">
              <div class="card-stat-label"><span>Class:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.class | slashToMaybeBreak }}</span>
              </div>
            </div>
            <!-- Attack -->
            <div v-if="cardData.attack" class="clearfix">
              <div class="card-stat-label"><span>Attack:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.attack }}</span>
              </div>
            </div>
            <!-- Armor Class -->
            <div v-if="cardData.armorClass" class="clearfix">
              <div class="card-stat-label"><span>Armor Class:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.armorClass }}</span>
              </div>
            </div>
            <!-- Skill -->
            <div v-if="cardData.skill" class="clearfix">
              <div class="card-stat-label"><span>Skill:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.skill }}</span>
              </div>
            </div>
            <!-- Hit Points -->
            <div v-if="cardData.hitPoints" class="clearfix">
              <div class="card-stat-label">
                <span>Hit Points:</span>
              </div>
              <div class="card-stat-value">
                <span>{{ cardData.hitPoints }}</span>
              </div>
            </div>
            <!-- Faction -->
            <div v-if="cardData.faction" class="clearfix">
              <div class="card-stat-label"><span>Faction:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.faction | slashToMaybeBreak }}</span>
              </div>
            </div>
            <!-- Traits -->
            <div v-if="cardData.traits" class="clearfix">
              <div class="card-stat-label"><span>Traits:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.traits | slashToLineBreak }}</span>
              </div>
            </div>
            <!-- Feats -->
            <div v-if="cardData.feats" class="clearfix">
              <div class="card-stat-label"><span>Feats:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.feats | slashToLineBreak }}</span>
              </div>
            </div>
            <!-- Misc -->
            <div v-if="cardData.misc" class="clearfix">
              <div class="card-stat-label"><span>Misc:</span></div>
              <div class="card-stat-value">
                <span>{{ cardData.misc | slashToLineBreak }}</span>
              </div>
            </div>
            <!-- Editions -->
            <div class="clearfix my-2">
              <div class="card-stat-label"><span>Formats:</span></div>
              <div v-if="cardData.editions && cardData.editions[0]" class="card-stat-value">
                <span v-for="edition in cardData.editions" :key="edition">{{ edition + " " }}</span>
              </div>
              <div v-else class="card-stat-value"><span>Open</span></div>
            </div>
            <!-- Text -->
            <div class="my-3" v-html="$options.filters.formatCardText(cardData.text)"></div>
            <!-- Print Info -->
            <div v-for="(printInfo, i) in cardData.printInfos" :key="i">
              <!-- Image Link (Set, Set Number) -->
              <div class="card-print-link" @click="setImage(printInfo.imageUrl)">
                <span>{{ printInfo | formatSetName }}</span>
              </div>
              <div class="mx-2">
                <!-- Rarity -->
                <div v-if="printInfo.rarity" class="clearfix">
                  <div class="card-stat-label"><span>Rarity:</span></div>
                  <div class="card-stat-value">
                    <span>{{ printInfo.rarity }}</span>
                  </div>
                </div>
                <!-- Flavor Traits -->
                <div v-if="printInfo.flavorTraits" class="clearfix">
                  <div class="card-stat-label"><span>Flavor Traits:</span></div>
                  <div class="card-stat-value">
                    <span>{{ printInfo.flavorTraits }}</span>
                  </div>
                </div>
                <!-- Artist -->
                <div v-if="printInfo.artist" class="clearfix">
                  <div class="card-stat-label"><span>Artist:</span></div>
                  <div class="card-stat-value">
                    <span>{{ printInfo.artist }}</span>
                  </div>
                </div>
                <!-- Flavor Text -->
                <div class="my-2">
                  <i>{{ printInfo.flavorText }}</i>
                </div>
              </div>
            </div>
          </div>
        </b-col>
      </b-row>
      <!-- Errata -->
      <div v-if="cardData.errata" class="my-3">
        <div class="font-weight-bold"><span>Rulings:</span></div>
        <div class="card-errata">
          <span>{{ cardData.errata }}</span>
        </div>
      </div>
    </template>
  </b-container>
</template>

<script>
import addRemoveCardMixin from "@/mixins/addRemoveCardMixin.js";

export default {
  name: "CardDetail",
  mixins: [addRemoveCardMixin],
  props: {
    card: String,
  },
  data() {
    return {
      imageUrlOverride: null,
    };
  },
  computed: {
    cardIndex() {
      return this.$store.state.cardIndex;
    },
    cardData() {
      return this.cardIndex[this.card] || {};
    },
    showSideMenus() {
      return this.$store.getters.showSideMenus;
    },
    imageUrl() {
      if (!this.cardData) return null;
      if (!this.cardData.printInfos) return null;
      let printInfos = this.cardData.printInfos.filter((x) => x.imageUrl);
      if (!printInfos[0]) return null;
      return printInfos[0].imageUrl;
    },
  },
  filters: {
    formatCardText(value) {
      if (!value) return value;
      let hashReg = /(Spend Order:|Order:|Spend React:|React:)/gm;
      value = value.replace(hashReg, "<b>$&</b>");
      hashReg = /\r\n/gm;
      value = value.replace(hashReg, "<br>");
      return value;
    },
    slashToLineBreak(value) {
      if (!value) return value;
      let hashReg = /\//gm;
      value = value.replace(hashReg, "\r\n");
      return value;
    },
    slashToMaybeBreak(value) {
      if (!value) return value;
      let hashReg = /\//gm;
      value = value.replace(hashReg, "/\u200B");
      return value;
    },
    formatSetName(value) {
      let display = value.set;
      if (value.setNumber != null) display += ` (${value.setNumber})`;
      return display;
    },
  },
  watch: {
    card() {
      this.imageUrlOverride = null;
    },
  },
  mounted() {
    this.$store.dispatch("loadCardData").then(() => {
      if (!this.cardIndex[this.card]) {
        this.$store.commit("setShow404", true);
      }
    });
  },
  methods: {
    setImage(imageUrl) {
      if (!imageUrl) return;
      this.imageUrlOverride = imageUrl;
    },
  },
};
</script>

<style scoped>
.card-image {
  position: sticky;
  top: 5px;
  max-height: 400px;
  max-width: 350px;
}

.card-stat-label {
  float: left;
  font-weight: bold;
  text-align: left;
  width: 35%;
}

.card-stat-value {
  float: right;
  white-space: pre-wrap;
  width: 64%;
}

.card-print-link {
  text-decoration: underline;
  cursor: pointer;
}
.card-print-link:hover {
  text-decoration: none;
}

.card-errata {
  white-space: pre-wrap;
}
</style>