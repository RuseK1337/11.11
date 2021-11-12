<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">

        <!-- table -->
        <table>

          <!-- head -->
          <thead>
            <tr>
              <th @click="sort('name')">Name &#8595;</th>
              <th @click="sort('age')">Age &#8595;</th>
              <th @click="sort('gender')">Gender &#8595;</th>
            </tr>
          </thead>

          <!-- body -->
          <tbody>
            <tr v-for="user in usersSort" :key="user.id">
              <td>
                <img :src="user.img" :alt="user.name">
                <span>{{ user.name }}</span>
              </td>
              <td> {{ user.age }} </td>
              <td> {{ user.gender }} </td>
            </tr>
          </tbody>

        </table>
        <p style="text-align:center;">
          <span> debug: sort: {{ currentSort }}, dir: {{ currentSortDir }} </span>
          <span> page: {{ this.page.current }}, length: {{ this.page.length }} </span>
        </p>
      </div>
    </section>

    <!-- buttons -->
    <section>
      <div class="contaier">
        <div class="button-list">
          <div class="btn btnPrimary" @click="prevPage"> &#8592; </div>
          <div class="btn btnPrimary" @click="nextPage"> &#8594; </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      users: [{
    "name": "Ivanka",
    "age": 25,
    "img": "https://i0.wp.com/prostolike.net/wp-content/uploads/2020/07/eb3fa351d40b7cc1d2b406af660267d0-ivanka-trump-gala.jpg?w=679&ssl=1",
    "gender": "female"
  },
  {
    "name": "John",
    "age": 45,
    "img": "https://newhaircutstyle.com/wp-content/uploads/2018/12/Actor-John-Krasinski-style-Haircut-20187.jpg",
    "gender": "male"
  },
  {
    "name": "Fedor",
    "age": 44,
    "img": "https://funik.ru/wp-content/uploads/2020/03/d52fdd4dbccc7056ab22.jpg",
    "gender": "male"
  },
  {
    "name": "Jack",
    "age": 13,
    "img": "https://i08.fotocdn.net/s127/7bb8a8a78843ef0f/user_l/2883005470.jpg",
    "gender": "male"
  },
  {
    "name": "Jane",
    "age": 25,
    "img": "https://i.pinimg.com/736x/2b/54/d7/2b54d769245c6157d5381409b6c3b151--jane-levy-short-necklace.jpg",
    "gender": "female"
  },
  {
    "name": "Niklaus",
    "age": 25,
    "img": "https://lh5.googleusercontent.com/-nepxj00KVhY/TXylgggKnvI/AAAAAAAAAIk/BKdaGYud0GA/s1600/niklaus_wirth_big.jpg",
    "gender": "male"
  }],
      currentSort: 'name',
      currentSortDir: 'asc',
      page: {
        current: 1,
        length: 4
      }
    }
  },
  created () {
    axios
      .get('https://api.myjson.com/bins/rzgya')
        .then(response => {
          // console.log(response.data)
          this.users = response.data
        })
        .catch(error => {
          console.log(error)
        })
  },
  computed: {
    usersSort () {
      return this.users.sort((a, b) => {
        let mod = 1
        if (this.currentSortDir === 'desc') mod = -1
        if (a[this.currentSort] < b[this.currentSort]) return -1 * mod
        if (a[this.currentSort] > b[this.currentSort]) return 1 * mod
        return 0
      }).filter((row, index) => {
        let start = (this.page.current-1)*this.page.length
        let end = this.page.current * this.page.length
        if (index >= start && index < end) return true
      })
    }
  },
  methods: {
    sort (e) {
      if (e === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
      }
      this.currentSort = e
    },
    // Pagination
    prevPage () {
      if (this.page.current > 1) this.page.current-=1
    },
    nextPage () {
      if ((this.page.current * this.page.length) < this.users.length) this.page.current+=1
    }
  }
}
</script>


<style lang="scss" scoped>
img {
  width: 60px;
  height: auto;
  border-radius: 50%;
  margin-right: 16px;
}
.button-list {
  width: 100%;
  text-align: center;

  .btn {
    border-radius: 60px;
    margin: 0 20px;
  }
}

</style>
