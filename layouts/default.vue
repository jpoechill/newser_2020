<template>
  <div>
    <no-ssr>            
      <div style="position: fixed; width: 100%; z-index: 100; margin-top: 40px;">
        <!-- <button @click="genOpacity = 0">Opacity 0</button>
        <button @click="genOpacity = .5">Opacity 50</button>
        <button @click="genOpacity = 1">Opacity 100</button> -->
        <div style="width: 100%; padding: 0px 100px;">
          <div style="display: inline;">
            <div style="display: inline-block; background-color: white; border-radius: 100px; height: 33px; width: 33px; margin-top: -4px; margin-right: 8px;"></div> 
            <div class="mobile-only" style="letter-spacing: 3px; font-size: 23px; vertical-align: top;"><b>NEWSER</b></div>
          </div>
          <div style="display: inline; float: right; padding-right: 200px;">
            <span class="fake-link" @click="getArticles('mtv-news')" style="margin-right: 40px; padding-bottom: 6px; border-bottom: 3px solid #FFFFFF;">MTV</span> 
            <span class="fake-link" style="margin-right: 40px;" @click="getArticles('the-verge')">THE VERGE</span>
            <span class="fake-link" style="margin-right: 0px;" @click="getArticles('polygon')">POLYGON</span> 
          </div>
        </div>
      </div>

      <!-- Start FullPage Content -->
      <full-page ref="fullpage" :options="options" id="fullpage">
        <div class="section fp-section fp-table" v-for="(article, index) in articles" :key="index" :style="{ backgroundColor: bgColors[index], height: docHeight }">
          <transition name="v-fade" mode="out-in">
            <div :style="{ opacity: genOpacity }" style="text-align: left; padding: 0px 100px; display: table-cell; vertical-align: middle; transition: opacity 0.3s; -webkit-transition: opacity 0.3s;">
                <div style="padding: 0px">
                  <div class="progressive-cover" style="margin-top: 0px; margin-bottom: 30px;">
                    <img :src="article.urlToImage" alt=""  class="headerImg" style="margin: auto; border-radius: 10px;">
                  </div>
                  <span class="heading">
                    {{ article.title }}
                  </span><br><br>
                  <span class="fill">
                    {{ article.description }}
                  </span>
                  <br><br>
                  <span>
                    {{ convertDate(article.publishedAt) }}
                  </span>
                  <span style="float: right;">
                    → Visit Story
                  </span>
                </div>
            </div>
          </transition>
        </div>
      </full-page>
    </no-ssr>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  created () {
    this.getArticles('polygon')
  },
  mounted() {
    this.docHeight = window.innerHeight + 'px'

    this.populateColors()
  },
  computed: {
    articlesComputed: function () {
      return [1,1,1,1]
    }
  },
  methods: {
    hasLoaded: function  () {
      console.log('Image has loaded')
    },
    getArticles: function (source) {
      let self = this

      this.makeVisible()

      self.genOpacity = 0.0

      let articles = []

      axios.get('https://cors-anywhere.herokuapp.com/https://newsapi.org/v2/everything?sources=' + source + '&apiKey=36dbfb425292424e94d13d7887c9e7ba', {
        headers: {
          'X-Requested-With': 'XMLHttpRequest'
        }
      })
      .then(response => {
        articles = response.data.articles
        
        self.articles = articles

        this.oogabooga = 'dog'
      })
      .catch(function () {
        console.log('ERR')
      })
      
      console.log('data is set')
    },
    addPage: function () {
      this.$forceUpdate(); 
      this.pages.push(1)
    },
    makeVisible() {
      let self = this
      
      setTimeout(function () {
        self.genOpacity = 1
        self.oogabooga = 'cat'
        
        console.log('data is complete')
      }, 1200)
    },
    populateColors: function () {
      for (let i = 0; i < 100; i++) {
        let color = this.getRandomPastel()
        console.log('color: ' + color)
        this.bgColors.push(this.getRandomPastel())
      }
    },
    convertDate(inputDate) {
      var date = new Date(inputDate);
      var weekday = ["Sunday", "Monday" ,"Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
      var months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

      return weekday[date.getDay()] + ' @ ' +  date.getHours() + ':' + date.getMinutes()
    },
    getRandomPastel: function () {
      let colors = [
        '#ADE6D0', 
        '#b7b6a1', 
        '#c5a2c7', 
        '#DEB3EB', 
        '#B4C6DD', 
        '#96d8d2', 
        '#F0B2D3', 
        '#c599c7']
      
      let randomNum = Math.floor(Math.random() * Math.floor(colors.length));
      
      return colors[randomNum]
    }
  },
  data() {
    return {
      pages: [1,1],
      oogabooga: 'cat',
      genOpacity: 0,
      showNews: false,
      docHeight: '1000px',
      API_Key: '86fa2caa5dac471a98d05dfa2d141b6f',
      articles: [{}],
      bgColors: [],
      sources: [
        {
          name: 'MTV News',
          title: 'MTV',
          slug: 'mtv-news',
          visible: false
        },
        {
          name: 'The Verge',
          title: 'VERGE',
          slug: 'the-verge',
          visible: true
        },
        {
          name: 'Polygon',
          title: 'POLYGON',
          slug: 'polygon',
          visible: true
        },
      ],
      options: {
        licenseKey: 'YOUR_KEY_HEERE',
        menu: '#menu',
        // sectionsColor: ['#ADE6D0', '#F1F0CF', '#EBCEED', '#DEB3EB', '#B4C6DD', '#DAF0EE', '#F0B2D3', '#EFDEF0']
      },
    }
  }
}
</script>

<style>
html {
  font-family: Arial, Helvetica;
  font-size: 20px;
  color: #FFFFFF;
}

.progressive-cover {
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
/* h1 {
  font-size: 180px;
} */

img {
  -webkit-box-shadow: 3px 8px 16px 2px rgba(0,0,0,0.23); 
  box-shadow: 3px 8px 16px 2px rgba(0,0,0,0.23);
}

/* For mobile phones: */
.mobile-only {
  display: none;
}

.heading {
  font-size: 40px;
  font-weight: 600;
}

.fill {
  font-size: 20px;
  line-height: 30px;
  font-weight: 400;
}

.headerImg {
  height: 100px;
}

@media only screen and (min-width: 600px) {
  /* For tablets: */  
  .mobile-only {
    display: inline-block;
  }

  .heading {
    font-size: 60px;
    font-weight: 600;
  }

  .fill {
    font-size: 20px;
    line-height: 30px;
    font-weight: 400;
  }  

  .headerImg {
    height: 160px;
  }
}

@media only screen and (min-width: 941px) {
  /* For desktop: */
  .mobile-only {
    display: inline-block;
  }

  .heading {
    font-size: 80px;
    font-weight: 600;
  }

  .fill {
    font-size: 24px;
    line-height: 30px;
    font-weight: 400;
  }
  
  .headerImg {
    height: 300px;
  }
}

.fake-link:hover {
  cursor: pointer;
}

.v-fade-enter-active, .v-fade-leave-active {
  transition: opacity .8s;
}
.v-fade-enter, .v-fade-leave-to {
  opacity: 0;
}
</style>
