<template>
  <div>
  
  <table>
    <thead>
      <tr>
        <th v-for="(key, index) in columns" :key="index"
          @click="sortBy(key)"
          :class="{ active: sortKey == key }">
          {{ key |   capitilize }}
          <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'dsc'">
          </span>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(entry, index) in filteredHeroes"  :key="index">
        <td v-for="key in columns" :key="key">
          {{entry[key]}}
        </td>
      </tr>
    </tbody>
  </table>

<hr>
{{sortKey}}
<hr>
  {{sortOrders}}

  </div>
</template>

<script>

export default {
    props:{
      heroes: Array,
      columns: Array,
      filterKey: String
    },
   data: function () {
    var sortOrders = {}
    this.columns.forEach(function (key) {
      console.log(key)
      sortOrders[key] = 1
    })
    return {
      sortKey: '',
      sortOrders: sortOrders
    }
  },
  computed: {
    filteredHeroes: function () {
      var sortKey = this.sortKey
      var filterKey = this.filterKey && this.filterKey.toLowerCase()
      var order = this.sortOrders[sortKey] || 1
      var heroes = this.heroes
      if (filterKey) {
        heroes = heroes.filter(function (row) {
          return Object.keys(row).some(function (key) {
            return String(row[key]).toLowerCase().indexOf(filterKey) > -1
          })
        })
      }
      if (sortKey) {
        heroes = heroes.slice().sort(function (a, b) {
          a = a[sortKey]
          b = b[sortKey]
          return (a === b ? 0 : a > b ? 1 : -1) * order
        })
      }
      return heroes
    }
  },
    methods: {
      sortBy: function (key) {
      this.sortKey = key
      this.sortOrders[key] = this.sortOrders[key] * -1
    }
    },

    filters:{
      capitilize(value){
        return  value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
}
</script>

<style scoped>

</style>