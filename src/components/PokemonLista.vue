<template>
  <div id="pokemon">
    <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image">
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{pokemon.id}} - {{upper(name)}}</p>
            <p class="subtitle is-6">{{pokemon.type}}</p>
          </div>
        </div>
        <div class="content">
          <button class="button is-medium is-fullwidth" @click="mudarSprite">Dar uma voltinha!</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
      return {
        isFront: true,
        currentImg: '',
        pokemon: {
          type: '',
          front: '',
          back: '',
          id: 0
        }
      }
    },
    created: function() {
      axios.get(this.url).then(res => {
        this.pokemon.type = this.upper(res.data.types[0].type.name);
        this.pokemon.front = res.data.sprites.front_default;
        this.pokemon.back = res.data.sprites.back_default;
        this.pokemon.id = res.data.id;
        this.currentImg = this.pokemon.front;
      })
    },
    props: {
      num: Number,
      name: String,
      url: String
    },
    methods: {
      upper: function(value) {
        var newName = value[0].toUpperCase() + value.slice(1);
        return newName;
      },
      mudarSprite: function() {
        if(this.isFront) {
          this.currentImg = this.pokemon.back;
          this.isFront = false;
        } else {
          this.currentImg = this.pokemon.front;
          this.isFront = true;
        }
      }
    }
}
</script>

<style>
#pokemon {
  margin-top: 2%;
}
</style>