<template>
  <div class="typewriterin" v-bind:style="backgroundStyle">
    <div class="typewriterin__header">
      <div class="typewriterin__header__logo">
        TYPEWRITERIN
      </div>
    </div>
    <div class="typewriterin__textarea">
      <textarea v-bind:style="editorStyle"></textarea>
    </div>
    <div class="typewriterin__footer" v-on:mouseover="toggleConfigButtons(true)" v-on:mouseleave="toggleConfigButtons(false)">
      <div class="typewriterin__footer__config-button-wrapper icon-button">
        <button class="typewriterin__footer__config-button" v-on:click="toggleConfigPanel(true)">
          <i class='fas fa-cog fa-3x'></i>
        </button>
      </div>
    </div>
    <div class="typewriterin__config">
      <div class="typewriterin__config__panel">
        <div class="typewriterin__config__panel__row">
          <label for="typewriterin-font-family">FONT-FAMILY: </label>
          <div class="input-wrapper">
            <input type="text" name='typewriterin-font-family' :value="editorFontFamily"  v-bind:style="configStyle"/>
          </div>
        </div>
        <div class="typewriterin__config__panel__row">
          <label for="typewriterin-font-size">FONT-SIZE: </label>
          <div class="input-wrapper">
            <input type="text" name='typewriterin-font-size' :value="editorFontSize"  v-bind:style="configStyle"/>
          </div>
        </div>
        <div class="typewriterin__config__panel__row">
          <label for="typewriterin-font-color">FONT-COLOR: </label>
          <div class="input-wrapper">
            <input type="text" name='typewriterin-font-color' :value="editorFontColor"  v-bind:style="configStyle"/>
          </div>
        </div>
        <div class="typewriterin__config__panel__row">
          <label for="typewriterin-background-color">BACKGROUND-COLOR: </label>
          <div class="input-wrapper">
            <input type="text" name='typewriterin-background-color' :value="editorBackGround"   v-bind:style="configStyle"/>
          </div>
        </div>
        <div class="typewriterin__config__panel__close-button-wrapper icon-button">
          <button class="typewriterin__config__panel__close-button white" v-on:click="toggleConfigPanel(false); updateConfig();">
            <i class='fas fa-times fa-3x'></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { gsap, TweenMax } from 'gsap';
  export default {
    data: function () {
      return {
        editorFontFamily: 'NitalagoRuika-03',
        editorFontColor: '#eee',
        editorFontSize: '1.2rem',
        editorBackGround: '#333'
      }
    },

    mounted: function() {
      window.onload = function() {

        var logo = document.querySelector('.typewriterin__header__logo')
        console.log('aaa')
        TweenMax.to(logo, 0.5, {
          delay: 1,
          opacity: 0,
        });
      }
    },

    computed: {
      editorStyle: function() {
        return {
          'font-family': this.editorFontFamily,
          'color': this.editorFontColor,
          'font-size': this.editorFontSize
        }
      },
      backgroundStyle: function() {
        return {
          'color': this.editorFontColor,
          'background': this.editorBackGround
        }
      },

      configStyle: function() {
        return {
          'color': this.editorFontColor,
        }
      }
    },
    methods: {
      toggleConfigButtons: function(is_show) {
        var opacity = 0
        var marginTop = 0
        if (is_show)
          opacity = 1
        if (!is_show)
          marginTop = '5rem'

        var configButtons = document.querySelectorAll(".typewriterin__footer__config-button");
        configButtons.forEach(function(element, index) {
          TweenMax.to(element, 0.3, {
            opacity: opacity,
            'margin-top': marginTop,
          });
        });
      },
      toggleConfigPanel: function(is_show) {
        var vue = this
        var configPanel = document.querySelector(".typewriterin__config");
        var opacity = 0
        if (is_show) {
          configPanel.style.visibility = 'visible'
          opacity = 1
        }

        TweenMax.to(configPanel, 0.3, {
          opacity: opacity,
          onComplete: function() {
            if (!is_show) {
              configPanel.style.visibility = 'hidden'
            }
          }
        });
      },
      updateConfig: function() {
        this.editorFontFamily = document.querySelector("input[name=typewriterin-font-family]").value;
        this.editorFontSize = document.querySelector("input[name=typewriterin-font-size]").value;
        this.editorFontColor = document.querySelector("input[name=typewriterin-font-color]").value;
        this.editorBackGround = document.querySelector("input[name=typewriterin-background-color]").value;
      }
    }
  }
</script>

<style lang='scss' scoped>
.typewriterin {
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  position: absolute;

  * {
    outline: none;
  }

  .typewriterin__header {
    width: 100vw;
    height: 5rem;

    .typewriterin__header__logo {
      font-size: 2rem;
      line-height: 5rem;
      text-align: center;
      font-family: 'NitalagoRuikaMono-06';
    }
  }

  .typewriterin__textarea {
    margin: 0 auto;
    padding: 2rem;
    width: 50vw;
    min-width: 10rem;
    border: 1px dotted #666;

    textarea {
      display: block;
      height: 30rem;
      background: none;
      outline: none;
      border: none;
      width: 100%;
    }
  }

  .typewriterin__footer {
    display: flex;
    flex-direction: row;
    justify-content: center;

    background: none;

    position: fixed;
    bottom: 0;
    left: 0;
    width: 100vw;
    height: 5rem;
  }

  .typewriterin__config {
    opacity: 0;
    visibility: hidden;
    position: fixed;
    left: 0;
    top: 0;

    background: rgba(0, 0, 0, .3);
    width: 100%;
    height: 100%;
    overflow: hidden;

    .typewriterin__config__panel {
      width: 80vw;
      min-height: 50vh;

      margin: 5rem auto;
      padding: 2rem 7rem;
      border-radius: 20px;

      background: rgba(0, 0, 0, .7);
      display: flex;
      flex-direction: column;
      justify-content: center;

      .typewriterin__config__panel__row {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 1rem 0;
        width: 100%;

        * {
          font-size: 1.5rem;
          font-family: 'NitalagoRuika-03';
        }

        label, .input-wrapper {
          display: block;
        }

        label {
          margin-top: .5rem;
        }

        .input-wrapper {

          background: #333;
          padding: .5rem 2rem;
          border-radius: 1rem;

          input {
            width: 20rem;
            background: none;
            border: none;
            outline: none;
            color: #eee;
          }
        }
      }

      .typewriterin__config__panel__row:not(:first-child) {
        border-top: 1px solid #eee;
      }

      .typewriterin__config__panel__close-button-wrapper {
        margin: 3rem auto;
      }
    }
  }


  .icon-button {

    button {
      color: #eee;
      border: none;
      cursor: pointer;
      margin-top: 5rem;

      border-radius: 50%;
      width: 4rem;
      height: 4rem;
      background: rgba(0, 0, 0, .3);
    }

    button.white {
      color: #333;
      background: #eee;
    }
  }

}
</style>
