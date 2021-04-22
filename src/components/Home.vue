<template>
  <div>
    <input type="text" v-model="inputSearch" v-on:change="fetchData" />
    <ul>
      <li v-for="(item, index) in arrayOfData" :key="item.name">
        <div># {{ index + 1 }}</div>
        <img class="resize" v-bind:src="item.owner.avatar_url" />
        <a v-bind:href="`https://${item.git_url.split('//')[1]}`">
          {{ item.name }}
        </a>
        <div>{{ item.owner.login }}</div>
        <div>{{ item.stargazers_count }} stars</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Home",
  data: function () {
    return {
      inputSearch: "javascript",
      arrayOfData: [],
    };
  },

  created() {
    this.fetchData();
    console.log("arrayOfData", this.arrayOfData);
  },

  methods: {
    fetchData() {
      const URL = `https://api.github.com/search/repositories?q=language:${this.inputSearch}&sort=stars`;
      fetch(URL)
        .then((response) => response.json())
        .then((data) => {
          this.arrayOfData = data.items;
        });
    },
  },
};
</script>

<style>
li {
  display: flex;
  flex-direction: column;
  list-style-type: none;
}
.resize {
  width: 100px;
  height: auto;
  border-radius: 50%;
}
</style>
