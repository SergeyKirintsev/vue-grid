<template>
  <div :style="gridAreas" class="grid-container">
    <div
      v-for="(item, idx) in headers"
      :class="[`${getLevelClass(0)} ${getLevelClass(0)}-${idx + 1}`]"
      :style="{ gridArea: `header-${idx + 1}` }"
    >
      {{ item }}
    </div>

    <div
      v-for="(item, idx) in level_1"
      :class="[`${getLevelClass(1)} ${getLevelClass(1)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(1)}-${idx + 1}` }"
    >
      <span>{{ item.name }}</span>
      <button class="trash-btn"></button>
    </div>

    <div
      v-for="(item, idx) in level_2"
      :class="[`${getLevelClass(2)} ${getLevelClass(2)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(2)}-${idx + 1}` }"
    >
      {{ item.name }}
    </div>

    <div
      v-for="(item, idx) in level_3"
      :class="[`${getLevelClass(3)} ${getLevelClass(3)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(3)}-${idx + 1}` }"
    >
      {{ item.name }}
    </div>

    <div
      v-for="(item, idx) in level_4"
      :class="[`${getLevelClass(4)} ${getLevelClass(4)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(4)}-${idx + 1}` }"
    >
      {{ item.name }}
    </div>

    <div
      v-for="(item, idx) in level_5"
      :class="[`${getLevelClass(5)} ${getLevelClass(5)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(5)}-${idx + 1}` }"
    >
      <span> {{ item.name }} {{ item.value }}</span>
      <button class="edit-btn"></button>
    </div>

    <div class="line line-1"><button class="edit-btn"></button></div>
    <div class="line line-2"><button class="edit-btn"></button></div>
    <div class="line line-3"><button class="edit-btn"></button></div>
    <div class="line line-4"></div>

    <div class="line-5"><button class="edit-btn"></button></div>
    <div class="line-6"><button class="edit-btn"></button></div>
    <div class="line-7"><button class="edit-btn"></button></div>
    <div class="line-8"></div>
  </div>
</template>

<script>
export default {
  name: 'TableJson',

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
        this.notId
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
        ).filter(this.notId);

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
          ).filter(this.notId);

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
              const keysLevelFive = Object.keys(current).filter(this.notId);

              const name = keysLevelFive[0];
              const value = current[name];
              const path = [
                keyLevelOne,
                keyLevelTwo,
                keyLevelThree,
                keyLevelFour,
              ];

              data.push({
                name,
                value,
                path,
              });
            }
          });
        });
      });

      return data;
    },

    gridAreas() {
      const areas = ['header-1  header-2   header-3   header-4  header-5'];

      const levels = {
        1: {
          index: 1,
          name: '',
        },
        2: {
          index: 1,
          name: '',
        },
        3: {
          index: 1,
          name: '',
        },
        4: {
          index: 1,
          name: '',
        },
        5: {
          index: 1,
          name: '',
        },
        line: {
          index: 1,
        },
      };

      const docs = this.level_5.map((el) => el.path);

      const arr0 = docs[0];
      levels[1].name = arr0[0];
      levels[2].name = arr0[1];
      levels[3].name = arr0[2];
      levels[4].name = arr0[3];

      docs.forEach((arr) => {
        let areasLine = '';

        for (let idx = 1; idx <= 4; idx++) {
          if (levels[idx].name !== arr[idx - 1]) {
            levels[idx].index++;
            levels[idx].name = arr[idx - 1];

            if (idx === 2) {
              areas.push(this.getLine(levels.line.index));
              levels.line.index++;
            }
          }
          areasLine += `${this.getLevelClass(idx)}-${levels[idx].index} `;
        }
        areasLine += `${this.getLevelClass(5)}-${levels[5].index} `;
        levels[5].index++;

        areas.push(areasLine.trim());
      });
      areas.push(this.getLine(levels.line.index));

      let areasText = '';

      areas.forEach((areasLine) => {
        areasText += `"${areasLine}" `;
      });

      return `grid-template-areas: ${areasText};`;
    },
  },

  methods: {
    mapName(arr) {
      return new Set(arr.map((el) => el.name));
    },

    notId(key) {
      return key !== 'id';
    },

    getLevelClass(level) {
      const classes = {
        0: 'header',
        1: 'doc-class',
        2: 'doc-type',
        3: 'doc-lang',
        4: 'doc-vid',
        5: 'doc-amount',
        line: 'line',
      };
      return classes[level];
    },

    getLine(idx) {
      const lineIdx = (idx - 1) * 4 + 1;
      const lineClass = this.getLevelClass('line');
      return `doc-class-1 ${lineClass}-${lineIdx} ${lineClass}-${
        lineIdx + 1
      } ${lineClass}-${lineIdx + 2} ${lineClass}-${lineIdx + 3}`;
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

.doc-amount {
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
