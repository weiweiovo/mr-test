<template>
  <div class="main-container">
    <div class="top-bar">
      <div class="left">
        <input type="text" v-model="countryName" placeholder="請輸入國家名稱" />
        <span class="sort" @click="isReverse = !isReverse"
          >{{ isReverse ? "反序" : "正序"
          }}<span :class="['triangle', { 'is-reverse': isReverse }]"></span
        ></span>
      </div>
      <div class="page-box">
        <button @click="changePage('prev')">PREV</button>
        <span>{{ this.currentPage }} / {{ this.totalPage }}</span>
        <button @click="changePage('next')">NEXT</button>
      </div>
    </div>
    <table>
      <thead>
        <tr>
          <th>國旗</th>
          <th>國家名稱</th>
          <th>２位國家代碼</th>
          <th>3位國家代碼</th>
          <th>母語名稱</th>
          <th>替代國家名稱</th>
          <th>國際電話區號</th>
        </tr>
      </thead>
      <tbody v-if="isLoading">
        <tr>
          <td colspan="7" class="text-center h-100">加載中...</td>
        </tr>
      </tbody>
      <tbody v-else-if="filterSearch.length === 0">
        <tr>
          <td colspan="7" class="text-center h-100">無符合資料...</td>
        </tr>
      </tbody>
      <tbody v-else>
        <tr
          v-for="(item, i) in filterSearch.slice(
            pageStart,
            pageStart + pageSize
          )"
          :key="i"
        >
          <td class="flag">{{ item.flag }}</td>
          <td class="link" @click="getPop(item)">{{ item.name }}</td>
          <td class="text-center">{{ item.cca2 }}</td>
          <td class="text-center">{{ item.cca3 }}</td>
          <td>{{ item.native }}</td>
          <td>
            <ul>
              <li v-for="(altSpellings, ii) in item.altSpellings" :key="ii">
                {{ altSpellings }}
              </li>
            </ul>
          </td>
          <td class="text-center">{{ item.idd }}</td>
        </tr>
      </tbody>
    </table>
    <PopModel :country="currentCountry" @close="closePop" />
  </div>
</template>

<script>
import PopModel from "./PopModel.vue";
export default {
  name: "HelloWorld",
  components: {
    PopModel,
  },
  data() {
    return {
      countryList: [],
      List: [],
      countryName: "",
      currentCountry: null,
      isReverse: false,
      pageSize: 25,
      currentPage: 1,
      isLoading: true,
    };
  },

  created() {
    let self = this;
    console.log(self);
    self.$http.get("https://restcountries.com/v3.1/all").then((response) => {
      if (response.data) {
        this.getList(response.data);
        this.isLoading = false;
        console.log("22", this.countryList);
      }
    });
  },
  computed: {
    filterSearch: function () {
      var self = this;
      if (self.isReverse) {
        return self.countryList
          .filter((item) => self.getName(item.name).includes(self.countryName))
          .sort(function (a, b) {
            var nameA = self.getName(a.name).toUpperCase(); // ignore upper and lowercase
            var nameB = self.getName(b.name).toUpperCase(); // ignore upper and lowercase
            if (nameA < nameB) {
              return -1;
            }
            if (nameA > nameB) {
              return 1;
            }
            return 0;
          })
          .reverse();
      }
      return self.countryList
        .filter((item) => item.name.includes(this.countryName))
        .sort(function (a, b) {
          var nameA = self.getName(a.name).toUpperCase(); // ignore upper and lowercase
          var nameB = self.getName(b.name).toUpperCase(); // ignore upper and lowercase
          if (nameA < nameB) {
            return -1;
          }
          if (nameA > nameB) {
            return 1;
          }
          return 0;
        });
    },
    pageStart() {
      return (this.currentPage - 1) * this.pageSize;
    },
    totalPage() {
      return Math.ceil(this.filterSearch.length / this.pageSize);
    },
  },
  methods: {
    getList(list) {
      const self = this;
      list.forEach((item) => {
        this.countryList.push({
          flag: item.flag,
          name: self.getName(item.name),
          cca2: item.cca2,
          cca3: item.cca3,
          native: self.getNativeName(item.name, item.languages),
          idd: item.idd.root,
          altSpellings: item.altSpellings,
        });
      });
    },
    getName(name) {
      if (name.official) {
        return name.official;
      }
      return "/";
    },
    getNativeName(name, languages) {
      if (languages) {
        let languagesId = Object.keys(languages);
        let langId = languagesId[0];
        if (name.nativeName[langId]) {
          return name.nativeName[langId].official;
        }
      }

      return "/";
    },
    changePage(type) {
      if (type === "prev") {
        if (this.currentPage > 1) {
          return this.currentPage--;
        }
      } else {
        if (this.currentPage < this.totalPage) {
          return this.currentPage++;
        }
      }
    },
    getPop(item) {
      this.currentCountry = item;
    },
    closePop() {
      this.currentCountry = null;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 0 10px;
}
.main-container {
  width: 1400px;
  margin: 0 auto;
}
.top-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
input {
  padding: 4px;
}
.sort {
  position: relative;
  margin: 0 1em;
  cursor: pointer;
}
.triangle {
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 4px;
  border-style: solid;
  border-width: 5px 5px 0 5px;
  border-color: #736357 transparent transparent transparent;
  transition: 0.3s;
}
.is-reverse {
  transform: rotate(180deg);
}
.page-box {
  display: flex;
  align-items: center;
  justify-content: center;
}
.page-box button {
  margin: 0 1em;
  padding: 0.5em 1em;
  border-radius: 4px;
  color: #42b983;
  border: 1px solid;
  background: #fff;
  cursor: pointer;
}
.page-box button:hover {
  color: #fff;
  background: #42b983;
}
.page-box button:active {
  color: #fff;
  background: #349469;
}
.h-100 {
  height: 100px;
}
</style>
