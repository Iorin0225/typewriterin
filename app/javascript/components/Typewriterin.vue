<template>
  <div class="typewriterin" v-bind:style="backgroundStyle">
    <div class="typewriterin__header">
      <div class="typewriterin__header__logo" v-bind:style="logoStyle">
        TYPEWRITERIN
      </div>
    </div>
    <div class="typewriterin__textarea" v-on:mousemove="flashTextArea()">
      <textarea v-bind:style="editorStyle" placeholder="Type anything!"></textarea>
    </div>
    <div class="typewriterin__footer" v-on:mouseover="toggleConfigButtons(true)" v-on:mouseleave="toggleConfigButtons(false)">
      <div class="typewriterin__footer__config-button-wrapper icon-button">
        <button class="typewriterin__footer__config-button" v-on:click="toggleConfigPanel(true)" v-bind:style="iconStyle">
          <i class='fas fa-cog fa-3x'></i>
        </button>
      </div>
    </div>
    <div class="typewriterin__config">
      <div class="typewriterin__config__panel" v-bind:style="configPanelStyle">
        <div class="typewriterin__config__panel__row" v-bind:style="configBorderColorStyle">
          <label for="typewriterin-font-family">FONT-FAMILY: </label>
          <div class="input-wrapper">
            <input type="text" name='typewriterin-font-family' :value="editorFontFamily" v-bind:style="configPanelInputStyle" />
          </div>
        </div>
        <div class="typewriterin__config__panel__row" v-bind:style="configBorderColorStyle">
          <label for="typewriterin-font-size">FONT-SIZE: </label>
          <div class="input-wrapper">
            <input type="text" name='typewriterin-font-size' :value="editorFontSize" v-bind:style="configPanelInputStyle" />
          </div>
        </div>
        <div class="typewriterin__config__panel__row" v-bind:style="configBorderColorStyle">
          <label for="typewriterin-font-color">FONT-COLOR: </label>
          <div class="input-wrapper">
            <input type="text" name='typewriterin-font-color' :value="editorFontColor" v-bind:style="configPanelInputStyle" />
          </div>
        </div>
        <div class="typewriterin__config__panel__row" v-bind:style="configBorderColorStyle">
          <label for="typewriterin-background-color">BACKGROUND-COLOR: </label>
          <div class="input-wrapper">
            <input type="text" name='typewriterin-background-color' :value="editorBackGround" v-bind:style="configPanelInputStyle" />
          </div>
        </div>
        <div class="typewriterin__config__panel__close-button-wrapper icon-button">
          <button class="typewriterin__config__panel__close-button" v-on:click="toggleConfigPanel(false); updateConfig(); saveConfig();" v-bind:style="iconStyle">
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
        editorFontFamily: 'sans-serif',
        editorFontColor: '#ccc',
        editorFontSize: '1rem',
        editorBackGround: '#333'
      }
    },

    mounted: function() {
      this.loadConfig();

      window.onload = function() {
        let logo = document.querySelector('.typewriterin__header__logo')
        TweenMax.to(logo, 0.5, {
          delay: 1,
          opacity: 0,
        });

        let textarea = document.querySelector('.typewriterin__textarea')
        TweenMax.to(textarea, 0.5, {
          delay: 1,
          border: 'none',
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
      configPanelStyle: function() {
        return {
          'font-family': this.editorFontFamily,
          'color': this.editorFontColor,
          'font-size': this.editorFontSize,
          'background': this.editorBackGround
        }
      },
      configPanelInputStyle: function() {
        return {
          'color': this.editorFontColor
        }
      },
      configBorderColorStyle: function() {
        return {
          'border-color': this.editorFontColor
        }
      },
      iconStyle: function() {
        return {
          'color': this.editorBackGround,
          'background': this.editorFontColor
        }
      },
      logoStyle: function() {
        return {
          'color': this.editorFontColor,
        }
      }
    },
    methods: {
      flashTextArea: function() {
        let textarea = document.querySelector('.typewriterin__textarea')
        TweenMax.to(textarea, 0.1, {
          border: '1px dotted #666',
          onComplete: function() {
            TweenMax.to(textarea, 0.5, {
              delay: 1,
              border: 'none',
            });
          }
        });
      },
      toggleConfigButtons: function(is_show) {
        let opacity = 0
        let marginTop = 0
        if (is_show)
          opacity = 1
        if (!is_show)
          marginTop = '5rem'

        let configButtons = document.querySelectorAll(".typewriterin__footer__config-button");
        configButtons.forEach(function(element, index) {
          TweenMax.to(element, 0.3, {
            opacity: opacity,
            'margin-top': marginTop,
          });
        });
      },
      toggleConfigPanel: function(is_show) {
        let vue = this
        let configPanel = document.querySelector(".typewriterin__config");
        let opacity = 0
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
      },
      loadConfig: function() {
        let config = localStorage.getItem('typewriterin--config')
        if (!config) return

        config = JSON.parse(config)

        this.editorFontFamily = config.editorFontFamily
        this.editorFontColor = config.editorFontColor
        this.editorFontSize = config.editorFontSize
        this.editorBackGround = config.editorBackGround
      },
      saveConfig: function() {
        localStorage.setItem('typewriterin--config', JSON.stringify({
          editorFontFamily: this.editorFontFamily,
          editorFontColor: this.editorFontColor,
          editorFontSize: this.editorFontSize,
          editorBackGround: this.editorBackGround
        }))
      },
    }
  }
</script>

<style lang='scss' scoped>
.typewriterin {
  top: 0;
  left: 0;
  width: 100vw;
  min-height: 100vh;
  position: absolute;
  overflow: hidden;

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
      font-family: 'Arial Black', sans-serif;
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
      max-height: 80vh;
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
      max-height: 50vh;
      overflow: scroll;

      margin: 5rem auto;
      padding: 2rem 7rem;
      border-radius: 20px;

      // background: #333;
      display: flex;
      flex-direction: column;
      justify-content: start;

      .typewriterin__config__panel__row {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 1rem 0;
        width: 100%;

        * {
          font-size: 1.5rem;
          font-family: 'Arial Black';
        }

        label, .input-wrapper {
          display: block;
        }

        label {
          margin-top: .5rem;
        }

        .input-wrapper {
          padding: .5rem 2rem;
          border-radius: 1rem;

          input {
            width: 20rem;
            background: none;
            border: none;
            outline: none;
          }
        }
      }

      .typewriterin__config__panel__row:not(:first-child) {
        border-top: 1px solid;
      }

      .typewriterin__config__panel__close-button-wrapper {
        margin: 3rem auto;
      }
    }
  }


  .icon-button {

    button {
      border: none;
      cursor: pointer;
      margin-top: 5rem;

      border-radius: 50%;
      width: 4rem;
      height: 4rem;
    }
  }

}
</style>
