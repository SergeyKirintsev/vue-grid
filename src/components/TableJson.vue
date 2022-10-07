<template>
  <div>
    <h2>super table!!!</h2>
    <div :style="gridAreas" class="grid-container">
      <Header :items="headers" />

      <div
        v-for="(item, idx) in level_1"
        :style="{ gridArea: `doc-class` }"
        class="doc-class"
      >
        <span>{{ item.name }}</span>
        <button class="trash-btn"></button>
      </div>

      <div
        v-for="(item, idx) in level_2"
        :class="[`doc-type-${idx + 1}`]"
        :style="{ gridArea: `doc-type-${idx + 1}` }"
      >
        {{ item.name }}
      </div>

      <div
        v-for="(item, idx) in level_3"
        :class="[`doc-lang-${idx + 1}`]"
        :style="{ gridArea: `doc-lang-${idx + 1}` }"
      >
        {{ item.name }}
      </div>

      <div
        v-for="(item, idx) in level_4"
        :class="[`doc-vid-${idx + 1}`]"
        :style="{ gridArea: `doc-vid-${idx + 1}` }"
      >
        {{ item.name }}
      </div>

      <div
        v-for="(item, idx) in level_5"
        :class="['doc', `doc-${idx + 1}`]"
        :style="{ gridArea: `doc-${idx + 1}` }"
      >
        <span> {{ item.name }} {{ item.value }}</span>
        <button class="edit-btn"></button>
      </div>

      <div class="line line-1"></div>
      <div class="line line-2"><button class="edit-btn"></button></div>
      <div class="line line-3"><button class="edit-btn"></button></div>
      <div class="line line-4"></div>

      <div class="line-5"><button class="edit-btn"></button></div>
      <div class="line-6"><button class="edit-btn"></button></div>
      <div class="line-7"><button class="edit-btn"></button></div>
      <div class="line-8"></div>
    </div>
  </div>
</template>

<script>
import Header from './Header.vue';

export default {
  name: 'TableJson',

  components: {
    Header,
  },

  props: {
    obj: Object,
    headers: Array,
  },

  computed: {
    level_1() {
      const key = Object.keys(this.obj)[0];
      const id = this.obj[key].id;
      return [
        {
          id,
          name: key,
        },
      ];
    },

    level_2() {
      const keyLevelOne = this.level_1[0].name;
      const keysLevelTwo = Object.keys(this.obj[keyLevelOne]).filter(
        (key) => key !== 'id'
      );

      const data = [];

      keysLevelTwo.forEach((key) => {
        const id = this.obj[keyLevelOne][key].id;
        const name = key;
        data.push({
          id,
          name,
        });
      });

      return data;
    },

    level_3() {
      const keyLevelOne = this.level_1[0].name;
      const keysLevelTwo = this.mapName(this.level_2);

      const data = [];

      keysLevelTwo.forEach((keyLevelTwo) => {
        const keysLevelThree = Object.keys(
          this.obj[keyLevelOne][keyLevelTwo]
        ).filter((key) => key !== 'id');

        keysLevelThree.forEach((keyLevelThree) => {
          const id = this.obj[keyLevelOne][keyLevelTwo][keyLevelThree].id;
          const name = keyLevelThree;
          data.push({
            id,
            name,
          });
        });
      });

      return data;
    },

    level_4() {
      const keyLevelOne = this.level_1[0].name;
      const keysLevelTwo = this.mapName(this.level_2);
      const keysLevelThree = this.mapName(this.level_3);

      const data = [];

      keysLevelTwo.forEach((keyLevelTwo) => {
        keysLevelThree.forEach((keyLevelThree) => {
          const keysLevelFour = Object.keys(
            this.obj[keyLevelOne][keyLevelTwo][keyLevelThree]
          ).filter((key) => key !== 'id');

          keysLevelFour.forEach((keyLevelFour) => {
            const id =
              this.obj[keyLevelOne][keyLevelTwo][keyLevelThree][keyLevelFour]
                .id;
            const name = keyLevelFour;
            data.push({
              id,
              name,
            });
          });
        });
      });

      return data;
    },

    level_5() {
      const keyLevelOne = this.level_1[0].name;
      const keysLevelTwo = this.mapName(this.level_2);
      const keysLevelThree = this.mapName(this.level_3);
      const keysLevelFour = this.mapName(this.level_4);

      const data = [];

      keysLevelTwo.forEach((keyLevelTwo) => {
        keysLevelThree.forEach((keyLevelThree) => {
          keysLevelFour.forEach((keyLevelFour) => {
            const current =
              this.obj[keyLevelOne][keyLevelTwo][keyLevelThree][keyLevelFour];
            if (current) {
              const keysLevelFive = Object.keys(current).filter(
                (key) => key !== 'id'
              );

              const name = keysLevelFive[0];
              const value = current[name];

              data.push({
                name,
                value,
              });
            }
          });
        });
      });

      return data;
    },

    gridAreas() {
      const arr = [
        'header-1 header-2 header-3 header-4 header-5',
        'doc-class doc-type-1 doc-lang-1 doc-vid-1 doc-1',
        'doc-class doc-type-1 doc-lang-2 doc-vid-2 doc-2',
        'doc-class doc-type-1 doc-lang-2 doc-vid-3 doc-3',
        'doc-class doc-type-1 doc-lang-2 doc-vid-4 doc-4',
        'doc-class line-1     line-2     line-3    line-4',
        'doc-class doc-type-2 doc-lang-3 doc-vid-5 doc-5',
        'doc-class doc-type-2 doc-lang-4 doc-vid-6 doc-6',
        'doc-class line-5     line-6     line-7    line-8',
      ];

      let areas = '';

      arr.forEach(el => {
        areas += `"${el}" `;
      })

      return `grid-template-areas: ${areas};`;
    },

    gridStyle() {
      return {
        display: 'grid',
        border: '2px solid #d4dbec',
        gridTemplateColumns: `repeat(5, 1fr)`,
        gridTemplateRows: '52px',
        gridTemplateAreas: this.gridAreas,
      };
    },
  },

  methods: {
    mapName(arr) {
      return new Set(arr.map((el) => el.name));
    },

    gridAreasMethods() {
      const h5 = 'header-5';
      return 'qwqw xccvx';
    },
  },
};
</script>

<style>
.header {
  background: #d4dbec;
}

.grid-container {
  border: 1px solid #d4dbec;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: 50px;
  grid-auto-rows: 40px;
}

.edit-btn {
  width: 30px;
  height: 30px;
  background: transparent;
  background-image: url('../assets/edit.svg');
  background-repeat: no-repeat;

  border: none;
  outline: none;

  margin: 5px;
}

.trash-btn {
  width: 30px;
  height: 30px;
  background: transparent;
  background-image: url('../assets/trash.svg');
  background-repeat: no-repeat;

  border: none;
  outline: none;
  margin: 5px;
}

.doc {
  display: flex;
  justify-content: space-between;
}

.doc-class {
  border-right: 1px solid #d4dbec;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 5px;
}

.line {
  border-bottom: 1px solid #d4dbec;
}

.line-1 {
  grid-area: line-1;
}

.line-2 {
  grid-area: line-2;
}

.line-3 {
  grid-area: line-3;
}

.line-4 {
  grid-area: line-4;
}

.line-5 {
  grid-area: line-5;
}

.line-6 {
  grid-area: line-6;
}

.line-7 {
  grid-area: line-7;
}

.line-8 {
  grid-area: line-8;
}

.line-9 {
  grid-area: line-9;
}
</style>
