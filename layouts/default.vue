<template>
  <div>
    <no-ssr>
      <full-page ref="fullpage" :options="options" id="fullpage">
        <div class="section">
          aaaa
        </div>
        <div class="section">
          aaaa
        </div>
        <div class="section">
          aaaa
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
  methods: {
    hasLoaded: function  () {
      console.log('Image has loaded')
    },
    getArticles: function (source) {
      let self = this

      // for (let i = 0; i < 5; i++) {
      //   console.log( Math.random())
      //   let newObject = {  }

      //   // self.articles[0].name = Math.random()
      //   self.articles[i] = null
      //   // console.log(self.articles[index])
      //   self.articles.splice(i,1,Math.random())
      // }
      //   // console.log(item)
      //   //  = 
 
      //   // Object.assign(self.articles[index], { name: Math.random()})
      //   // setTimeout(function () {
      //   //   self.showNews = true
      //   // }, 1000)
      // // })

      this.makeVisible()

      self.genOpacity = 0.0
      // this.articles.length = 0

      let articles = []

      axios.get('https://cors-anywhere.herokuapp.com/https://newsapi.org/v2/everything?sources=' + source + '&apiKey=36dbfb425292424e94d13d7887c9e7ba', {
        headers: {
          "X-Requested-With": "XMLHttpRequest"
        }
      })
      .then(response => {
        articles = response.data.articles

        for (let i = 0; i <= articles.length; i++) {
          self.articles[i] = articles[i]
        }


        // this.articles[0].title = 'dog'
        this.oogabooga = 'dog'
      })

      
      console.log('data is set')
    },
    makeVisible() {
      let self = this
      
      setTimeout(function () {
        self.genOpacity = 1
        self.oogabooga = 'cat'
        // self.articles[0].title = 'cat'
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
      oogabooga: 'cat',
      genOpacity: 0,
      showNews: false,
      docHeight: '1000px',
      API_Key: '86fa2caa5dac471a98d05dfa2d141b6f',
      articles: [{ name: 'ooga booga', title: '' }],
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
        sectionsColor: ['#ADE6D0', '#F1F0CF', '#EBCEED', '#DEB3EB', '#B4C6DD', '#DAF0EE', '#F0B2D3', '#EFDEF0']
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

.heading {
  font-size: 80px;
  font-weight: 600;
}

.fill {
  font-size: 24px;
  line-height: 30px;
  font-weight: 400;
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
