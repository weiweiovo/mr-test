<template>
  <div class="pop" v-if="country">
    <div class="mask" @click="close()"></div>
    <div class="pop-box">
      <div class="flag">{{ country.flag }}</div>
      <div class="table-box">
        <table>
          <tr>
            <td>國家名稱</td>
            <td>{{ country.name }}</td>
          </tr>
          <tr>
            <td>2位國家代碼(cca2)</td>
            <td>{{ country.cca2 }}</td>
          </tr>
          <tr>
            <td>3位國家代碼(cca3)</td>
            <td>{{ country.cca3 }}</td>
          </tr>
          <tr>
            <td>母語名稱</td>
            <td>{{ country.native }}</td>
          </tr>
          <tr>
            <td>替代國家名稱</td>
            <td>
              <ul>
                <li
                  v-for="(altSpellings, ii) in country.altSpellings"
                  :key="ii"
                >
                  {{ altSpellings }}
                </li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>國際區碼</td>
            <td>{{ country.idd }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
// import axios from "axios";
export default {
  name: "PopModel",
  props: {
    country: Object,
    default: null,
  },
  data() {
    return {
      countryList: [],
      countryName: "",
      isReverse: false,
      pageSize: 25,
      currentPage: 1,
      isLoading: true,
    };
  },
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pop {
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
}
.mask {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6);
  z-index: -1;
}
.pop-box {
  width: 800px;
  height: 50%;
  padding: 50px;
  border-radius: 16px;
  background: #f1f1f1;
  display: flex;
  justify-content: space-around;
}
.table-box {
  width: 70%;
  max-height: 100%;
  border: 1px solid #919191;
  border-radius: 16px;
  overflow-y: auto;
}
table {
  height: 100%;
}
ul {
  padding-left: 20px;
}
</style>
