<template>
  <div>
    <div id='build-Container'>
      <p id='buildTitle'>Builds recomendadas</p>
      <div class="buildRow-Container">
        <div>
          <p id='itemTitle'>Starting Items</p>
          <div id='starting-Container'>
            <div v-for='(item,i) in build.startingItems' v-bind:key="i" v-show="item !== 0">
              <img :src="'/static/item_icons/'+ item +'.png'" class="itemImage">
            </div>
          </div>
        </div>
        <div>
          <p id='itemTitle'>Mythic Items</p>
          <div id='mythic-Container'>
            <div v-for='(item,i) in build.mythicItems' v-bind:key="i" v-show="item !== 0">
              <img :src="'/static/item_icons/'+ item +'.png'" class="itemImage">
            </div>
          </div>
        </div>
      </div>
      <div class="buildRow-Container">
        <div>
          <p id='itemTitle'>Boots Items</p>
          <div id='boots-Container'>
            <div v-for='(item,i) in build.bootsItems' v-bind:key="i" v-show="item !== 0">
              <img :src="'/static/item_icons/'+ item +'.png'" class="itemImage">
            </div>
            <div class="itemImage" v-show="(build.bootsItems[0]===0 && build.bootsItems[1]===0)">
            </div>
          </div>
        </div>
        <div>
          <p id='itemTitle'>Legendary Items</p>
          <div id='legendary-Container'>
            <div v-for='(item, i) in build.legendaryItems' v-bind:key="i" v-show="item !== 0">
              <img :src="'/static/item_icons/'+ item +'.png'" class="itemImage">
            </div>
          </div>
        </div>
      </div>
      <div class="buildRow-Container">
        <div>
          <p id='itemTitle'>Trinket Items</p>
          <div id='trinket-Container'>
            <div v-for='(item, i) in build.trinketItems' v-bind:key="i" v-show="item !== 0">
              <img :src="'/static/item_icons/'+ item +'.png'" class="itemImage">
            </div>
          </div>
        </div>
      </div>
      <div class="ability-Container" v-if="abilities !== 0">
        <div>
          Orden de habilidades
        </div>
        <div class="abilityOrder-Container">
          <span>
            {{ abilities.ability_order[0] }}
          </span>
          <img src="/static/useful_icons/arrow.png">
          <span>
            {{ abilities.ability_order[1] }}
          </span>
          <img src="/static/useful_icons/arrow.png">
          <span>
            {{ abilities.ability_order[2] }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['champ_id'],
  data() {
    return {
      abilities: 0,
      builds: [],
      build: {
        startingItems: [],
        mythicItems: [],
        legendaryItems: [],
        bootsItems: [],
        trinketItems: []
      }
    }
  },
  created() {
    this.getBuild()
    this.getAbilities()
  },
  watch: {
    champ_id(newvalue, oldvalue) {
      this.getBuild()
      this.getAbilities()
    }
  },
  methods: {
    getBuild () {
      const path = 'http://localhost:8000/build/' + this.champ_id
      axios.get(path)
        .then((res) => {
          this.build = res.data
          console.log(this.builds)
          this.adaptBuild(this.build)
        })
        .catch((error) => {
          console.error(error)
        })
    },
    adaptBuild (build) {
      this.build.startingItems = [build['starter_1'], build['starter_2'], build['starter_3']]
      this.build.mythicItems = [build['mythic_1'], build['mythic_2'], build['mythic_3']]
      this.build.legendaryItems = [build['legendary_1'], build['legendary_2'], build['legendary_3'], build['legendary_4']]
      this.build.trinketItems = [build['trinket_1'], build['trinket_2']]
      this.build.bootsItems = [build['boots_1'], build['boots_2']]
    },
    getAbilities () {
      const path = 'http://localhost:8000/ability_set/' + this.champ_id
      console.log(path)
      axios.get(path)
        .then((res) => {
          this.abilities = res.data
          console.log(this.abilities)
        })
        .catch((error) => {
          console.error(error)
        })
    }
  }
}
</script>

<style>
#build-Container {
  display: flex;
  flex-direction: column;
}
#starting-Container {
  display: flex;
  flex-direction: row;
  border: 3px solid;
  border-color: black;
  background-color: black;
  margin: 5px;
  width: fit-content;
}
#mythic-Container {
  display: flex;
  flex-direction: row;
  border: 3px solid;
  border-color: black;
  background-color: black;
  margin: 5px;
  width: fit-content;
}
#legendary-Container {
  display: flex;
  flex-direction: row;
  border: 3px solid;
  border-color: black;
  background-color: black;
  margin: 5px;
  width: fit-content;
}
#boots-Container {
  display: flex;
  flex-direction: row;
  border: 3px solid;
  border-color: black;
  background-color: black;
  margin: 5px;
  width: fit-content;
}
#trinket-Container {
  display: flex;
  flex-direction: row;
  min-width: 0px;
  border: 3px solid;
  border-color: black;
  background-color: black;
  margin: 5px;
  width: fit-content;
}

.itemImage {
  height: 40px;
  width: 40px;
  display: flex;
  border: 2px solid;
  border-color: gray;
  margin: 1px;
}

#itemTitle {
  margin: 0px;
  margin-left: 6px;
  text-align: left;
  color: black;
  font-weight: bolder;
  font-size: 18px;
}
#buildTitle {
  text-align: center;
  color: black;
  font-weight: bolder;
  font-size: 18px;
}

.buildRow-Container {
  display: flex;
  flex-direction: row;
  align-content: center;
  justify-content: center;
  align-items: center;
  margin-top: 10px;
}

.ability-Container {
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
  font-weight: bolder;
  font-size: 18px;
}

.abilityOrder-Container {
  display: flex;
  flex-direction: row;
  align-content: center;
  justify-content: center;
  align-items: center;
  margin-top: 5px;
}

.abilityOrder-Container > * {
  margin: 5px;
}

.abilityOrder-Container > img {
  width: 10px;
  height: 10px;
  transform: rotate(180deg);
}

</style>
