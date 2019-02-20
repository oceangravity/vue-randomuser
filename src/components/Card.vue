<template>
  <div class="rc-component">
    <div class="rc-component__profile-container">
      <div class="card-background"></div>
      <transition tag="div" name="card" appear>
        <div v-show="visible" class="wrapper">
          <div class="container">
            <div class="top" :style="{'background-image' : `url(${data.picture.large})`}"></div>
            <div class="bottom">
              <h1><span>{{data.name.first}} {{data.name.last}}</span></h1>
              <p>{{data.email}}</p>
            </div>
          </div>
          <div class="inside">
            <div class="icon"><span class="info-icon">i</span></div>
            <div class="contents">
              <span>City</span>
              <h1>{{data.location.city}}</h1>
              <span>Age</span>
              <h1>{{data.dob.age}}</h1>
              <span>Phone</span>
              <h1>{{data.phone}}</h1>
            </div>
          </div>
        </div>
      </transition>
    </div>
    <div class="rc-component__refresh-button-wrapper">
      <div @click="refresh">
        <span v-if="!loading">Refresh</span>
        <svg v-else xmlns="http://www.w3.org/2000/svg" width="38" height="38" viewBox="0 0 38 38" stroke="rgba(136,136,136,0.25)">
          <g fill="none" fill-rule="evenodd">
            <g transform="translate(1 1)" stroke-width="2">
              <circle stroke-opacity=".55" cx="18" cy="18" r="18"/>
              <path d="M36 18c0-9.94-8.06-18-18-18" transform="rotate(7.37784 18 18)">
                <animateTransform attributeName="transform" type="rotate" from="0 18 18" to="360 18 18" dur="1s" repeatCount="indefinite"/>
              </path>
            </g>
          </g>
        </svg>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      loading: false,
      visible: false,
      data: {
        picture: {
          large: ''
        },
        name: {
          first: '',
          last: ''
        },
        email: '',
        location: {
          city: ''
        },
        dob: {
          age: 0
        },
        phone: 0
      }
    }
  },
  mounted () {
    this.refresh()
  },
  methods: {
    refresh () {
      this.loading = true
      this.visible = false
      axios
        .get('https://randomuser.me/api/')
        .then(response => {
          this.data = response.data.results[0]
          this.loading = false
          this.visible = true
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

  .rc-component__profile-container{
    min-height: 500px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .rc-component {
    position: relative;
  }

  .card-background {
    position: absolute;
    height: 475px;
    width: 285px;
    background-color: rgba(13, 113, 212, 0.03);
    border-radius: 8px;
  }

  .rc-component__refresh-button-wrapper {
    display: flex;
    justify-content: center;
    margin-top: 15px;

    div {
      color: aliceblue;
      padding: 10px 20px;
      background-color: #a7aeb9;
      border-radius: 3px;
      cursor: pointer;
      user-select: none;
      transform: scale(0.95);
      display: flex;
      min-height: 56px;
      min-width: 100px;
      justify-content: center;
      align-items: center;
      font-size: 1.5em;
      line-height: 1em;

      &:hover {
        transform: scale(1);
        box-shadow: 5px 20px 30px rgba(0, 0, 0, 0.2);
      }
    }
  }

  .wrapper {
    width: 300px;
    height: 500px;
    background: white;
    margin: auto;
    position: relative;
    overflow: hidden;
    border-radius: 10px 10px 10px 10px;
    box-shadow: 0;
    transform: scale(0.95);
    transition: box-shadow 0.5s, transform 0.5s;

    &:hover {
      transform: scale(1);
      box-shadow: 5px 20px 30px rgba(0, 0, 0, 0.2);
    }

    .container {
      width: 100%;
      height: 100%;

      .top {
        height: 80%;
        width: 100%;
        background-color: #abb2b9;
        -webkit-background-size: 100%;
        -moz-background-size: 100%;
        -o-background-size: 100%;
        background-size: 100%;
        background-size: cover;
        background-repeat: no-repeat;
      }

      .bottom {
        width: 100%;
        height: 20%;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        transition: transform 0.5s;
        background: #2e3942;
        color: #b9d1d4;
        font-size: 1em;

        span {
          text-transform: capitalize;
        }

        h1 {
          margin: 0;
          font-size: calc(1em + 50%);
        }

        p {
          margin: 0;
          padding: 0;
        }

      }
    }

    .inside {
      z-index: 9;
      background: #92879B;
      width: 140px;
      height: 140px;
      position: absolute;
      top: -70px;
      right: -70px;
      border-radius: 0px 0px 200px 200px;
      transition: all 0.5s, border-radius 2s, top 1s;
      overflow: hidden;

      .icon {
        position: absolute;
        right: 80px;
        top: 82px;
        color: white;
        opacity: 1;

        .info-icon {
          padding: 10px;
          background-color: #1c3952;
          color: #dccccc;
          border-radius: 50%;
          width: 10px;
          height: 10px;
          max-height: 10px;
          display: flex;
          justify-content: center;
          align-items: center;
          text-transform: none;
          text-decoration: none;
          font-family: monospace;
          font-weight: 600;
        }
      }

      &:hover {
        width: 100%;
        right: 0;
        top: 0;
        border-radius: 0;
        height: 80%;

        .icon {
          opacity: 0;
          right: 15px;
          top: 15px;
        }

        .contents {
          opacity: 1;
          transform: scale(1);
          transform: translateY(0);
        }
      }

      .contents {
        padding: 16% 0;
        opacity: 0;
        transform: scale(0.5);
        transform: translateY(-200%);
        transition: opacity 0.2s, transform 0.8s;
        height: 76%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        h1 {
          margin: 0 0 20px 0;
          font-size: calc(1em + 50%);
        }

        p {
          margin: 0;
          padding: 0;
        }

      }
    }
  }

  .card-enter-active {
    animation: card-in .5s;
  }

  .card-leave-active {
    animation: card-out .5s;
  }

  @keyframes card-in {
    0% {
      opacity: 0;
      left: -20%
    }
    100% {
      opacity: 1;
      left: 0%
    }
  }

  @keyframes card-out {
    0% {
      opacity: 1;
      left: 0%
    }
    100% {
      opacity: 0;
      left: 20%
    }
  }
</style>
