<template>
  <div class="card shadow" style="min-height: 100vh">
    <div class="card-body">
      <h5 class="card-title display-5">
        {{ deckAndPlayer.deckName }}
        <a
          v-if="this.$route.fullPath.indexOf('decklist') == -1"
          href="#"
          class="btn"
          @click="
            this.$router.push('/decklist/' + this.$route.params.id + '/editor')
          "
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-pencil-fill col"
            viewBox="0 0 16 16"
          >
            <path
              d="M12.854.146a.5.5 0 0 0-.707 0L10.5 1.793 14.207 5.5l1.647-1.646a.5.5 0 0 0 0-.708zm.646 6.061L9.793 2.5 3.293 9H3.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.207zm-7.468 7.468A.5.5 0 0 1 6 13.5V13h-.5a.5.5 0 0 1-.5-.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.5-.5V10h-.5a.5.5 0 0 1-.175-.032l-.179.178a.5.5 0 0 0-.11.168l-2 5a.5.5 0 0 0 .65.65l5-2a.5.5 0 0 0 .168-.11z"
            />
          </svg>
        </a>
      </h5>

      <img :src="deckAndPlayer.imgUrl" class="rounded-circle mb-2" alt="" />
      <h6 class="card-subtitle mb-2 text-muted">
        {{ deckAndPlayer.playerName }}
      </h6>
      <div class="row">
        <div class="col">
          <h5 class="card-title">Main Deck</h5>
          <ul class="list-group list-group-flush my-3">
            <li
              class="list-group-item d-flex flex-row"
              v-for="(card, index) in this.mainDeck"
              :key="index"
            >
              {{ card.qty }}
              <a
                href="#"
                class="text-decoration-none tooltip2"
                @mouseover="
                  this.current = '';
                  this.getCardImageFromAPI(card.name).then((result) => {
                    this.current = result;
                  });
                "
                @click="this.$router.push('/card/' + card.name)"
                >{{ card.name
                }}<img class="tooltiptext" :src="this.current" :alt="card.name"
              /></a>
            </li>
          </ul>
          <h6 class="card-subtitle mb-2 d-flex flex-row-reverse fw-bold">
            {{ this.getCount(this.mainDeck) }} cards
          </h6>
        </div>

        <div class="col">
          <h5 class="card-title">Side Deck</h5>
          <ul class="list-group list-group-flush my-3">
            <li
              class="list-group-item d-flex flex-row"
              v-for="(card, index) in this.sideboard"
              :key="index"
            >
              {{ card.qty }}
              <a
                href="#"
                class="text-decoration-none tooltip2"
                @click="this.$router.push('/card/' + card.name)"
                @mouseover="
                  this.current = '';
                  try {
                    this.getCardImageFromAPI(card.name)
                      .then((result) => {
                        this.current = result;
                      })
                      .catch(() => {
                        this.current = '';
                      });
                  } catch {
                    this.current = '';
                  }
                "
                >{{ card.name }}
                <img class="tooltiptext" :src="this.current" :alt="card.name" />
              </a>
            </li>
          </ul>
          <h6 class="card-subtitle mb-2 d-flex flex-row-reverse fw-bold">
            {{ this.getCount(this.sideboard) }} cards
          </h6>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { ScryfallApi } from "@/api";
export default {
  data() {
    return {
      current: "",
    };
  },
  methods: {
    getCount(array) {
      let count = 0;
      array.forEach((element) => {
        count = count + parseInt(element.qty);
      });

      return count;
    },
    async getCardImageFromAPI(cardName) {
      try {
        let card = await ScryfallApi.getCardByName(cardName);
        card = card.data;
        let image = "";
        if (card.image_uris) image = card.image_uris.small;
        else image = card.card_faces[0].image_uris.small;
        return image;
      } catch {
        return "";
      }
    },
  },
  props: ["mainDeck", "sideboard", "deckAndPlayer"],
};
</script>
<style>
.tooltip2 {
  position: relative;
  display: inline-block;
}

.tooltip2 .tooltiptext {
  visibility: hidden;
  width: auto;
  background-color: none;
  color: black;
  text-align: center;
  border-color: black;
  border-radius: 6px;
  padding: 5px 0;
  position: absolute;
  z-index: 1;
  top: -5px;
  left: 110%;
}

.tooltip2 .tooltiptext::after {
  content: "";
  position: absolute;
  top: 100%;
  right: 100%;
  margin-top: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: transparent black transparent transparent;
}
.tooltip2:hover .tooltiptext {
  visibility: visible;
}
</style>
