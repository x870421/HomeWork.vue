<script setup></script>

<template>
  <table>
    <thead>
      <tr>
        <th>KEY</th>
        <th v-for="(number, index) in 9" :key="index">CELL{{ number }}</th>
      </tr>
    </thead>
    <tbody>
      <tr
        :class="{ selected: selected === dataIndex }"
        v-for="(item, dataIndex) in data"
        :key="dataIndex"
        @click="selected = selected === dataIndex ? null : dataIndex"
      >
        <!--  如果 點到已經選取的，就取消選取；點到其他的就選取-->
        <td>
          <span
            @click.stop="toggleStar(dataIndex)"
            class="star"
            :class="{ selected: starSelected.includes(dataIndex) }"
          ></span>
          {{ item.key }}
        </td>
        <td v-for="(cell, cellIndex) in 9" :key="cellIndex">
          {{ item['cell' + cell] }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  data() {
    return {
      data: [],
      selected: null,
      starSelected: [],
    };
  },
  beforeMount() {
    const urls = [
      '../data/data1.json',
      '../data/data2.json',
      '../data/data3.json',
    ];

    const urlJSONs = urls.map((u) => fetch(u).then((res) => res.json()));
    Promise.all(urlJSONs).then((res) => {
      const data = res[0];

      //   處理CELL8
      res[1].forEach((row) => {
        const num = row.key.slice(1);
        data[num].cell8 = row.cell8;
      });
      //   處理CELL9
      for (let i in res[2]) {
        const num = res[2][i].cell4.slice(1, -2);
        data[num].cell9 = res[2][i].cell9;
      }
      this.data = data;
    });
  },
  mounted() {},
  methods: {
    // 點選星星
    toggleStar(dataIndex) {
      const index = this.starSelected.indexOf(dataIndex);
      // 沒有點選過星星，就新增
      if (index === -1) {
        this.starSelected.push(dataIndex);
      } else {
        // 點選過就刪掉
        this.starSelected.splice(dataIndex, 1);
      }
    },
  },
};
</script>

<style scoped></style>
