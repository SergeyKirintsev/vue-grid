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
      <button class="trash-btn" @click="deleteClass(item)"></button>
    </div>

    <div
      v-for="(item, idx) in levelList[2]"
      :class="[`${getLevelClass(2)} ${getLevelClass(2)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(2)}-${idx + 1}` }"
    >
      {{ item.name }}
      <button class="trash-btn" @click="deleteClass(item)"></button>
    </div>

    <div
      v-for="(item, idx) in levelList[3]"
      :class="[`${getLevelClass(3)} ${getLevelClass(3)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(3)}-${idx + 1}` }"
    >
      {{ item.name }}
      <button class="trash-btn" @click="deleteClass(item)"></button>
    </div>

    <div
      v-for="(item, idx) in levelList[4]"
      :class="[`${getLevelClass(4)} ${getLevelClass(4)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(4)}-${idx + 1}` }"
    >
      {{ item.name }}
      <button class="trash-btn" @click="deleteClass(item)"></button>
    </div>

    <div
      v-for="(item, idx) in level_5"
      :class="[`${getLevelClass(5)} ${getLevelClass(5)}-${idx + 1}`]"
      :style="{ gridArea: `${getLevelClass(5)}-${idx + 1}` }"
    >
      <span> {{ item.name }} {{ item.value }}</span>
      <button class="edit-btn" @click="editDocuments(item)"></button>
    </div>

    <!-- строки с кнопками -->
    <template v-for="(item, idx) in levelList[2]">
      <div
        :class="[`${getLevelClass('line')} ${getLevelClass('line')}-${idx + 1}-1`]"
        :style="{ gridArea: `${getLevelClass('line')}-${idx + 1}-1` }"
      >
        <button v-if="isLastElement(idx, level_2)" class="edit-btn" @click="editType(item)"></button>
      </div>

      <div
        :class="[`${getLevelClass('line')} ${getLevelClass('line')}-${idx + 1}-2`]"
        :style="{ gridArea: `${getLevelClass('line')}-${idx + 1}-2` }"
      >
        <button class="edit-btn" @click="editLanguage(item)"></button>
      </div>

      <div
        :class="[`${getLevelClass('line')} ${getLevelClass('line')}-${idx + 1}-3`]"
        :style="{ gridArea: `${getLevelClass('line')}-${idx + 1}-3` }"
      >
        <button class="edit-btn" @click="editVid(item)"></button>
      </div>

      <div
        :class="[`${getLevelClass('line')} ${getLevelClass('line')}-${idx + 1}-4`]"
        :style="{ gridArea: `${getLevelClass('line')}-${idx + 1}-4` }"
      >
        <!-- <button class="edit-btn"></button> -->
      </div>
    </template>
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
      const keysLevelTwo = Object.keys(this.obj[keyLevelOne]).filter(this.notId);

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
        const keysLevelThree = Object.keys(this.obj[keyLevelOne][keyLevelTwo]).filter(this.notId);

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
          const current = this.obj[keyLevelOne][keyLevelTwo][keyLevelThree];
          if (current) {
            const keysLevelFour = Object.keys(current).filter(this.notId);

            keysLevelFour.forEach((keyLevelFour) => {
              const id = this.obj[keyLevelOne][keyLevelTwo][keyLevelThree][keyLevelFour].id;
              const name = keyLevelFour;
              data.push({
                id,
                name,
              });
            });
          }
        });
      });

      return data;
    },

    level_5() {
      const keyLevelOne = this.level_1[0].name;
      const keysLevelTwo = this.mapName(this.level_2).sort();
      const keysLevelThree = this.mapName(this.level_3).sort();
      const keysLevelFour = this.mapName(this.level_4).sort();

      const data = [];

      keysLevelTwo.forEach((keyLevelTwo) => {
        keysLevelThree.forEach((keyLevelThree) => {
          keysLevelFour.forEach((keyLevelFour) => {
            const current = this.obj[keyLevelOne][keyLevelTwo]?.[keyLevelThree]?.[keyLevelFour];
            if (current) {
              const keysLevelFive = Object.keys(current).filter(this.notId);

              const name = keysLevelFive[0];
              const value = current[name];
              const path = [keyLevelOne, keyLevelTwo, keyLevelThree, keyLevelFour];

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

    levelList() {
      const docs = this.level_5.map((el) => el.path);

      const unique = new Set();

      const list = {
        2: [],
        3: [],
        4: [],
      };

      // 2
      docs.forEach((arrPath) => {
        const keyLevelOne = arrPath[0];
        const keyLevelTwo = arrPath[1];
        if (!unique.has(keyLevelOne + keyLevelTwo)) {
          const id = this.obj[keyLevelOne][keyLevelTwo].id;
          unique.add(keyLevelOne + keyLevelTwo);
          list[2].push({
            id,
            name: keyLevelTwo,
            path: [keyLevelOne, keyLevelTwo],
          });
        }
      });

      // 3
      docs.forEach((arrPath) => {
        const keyLevelOne = arrPath[0];
        const keyLevelTwo = arrPath[1];
        const keyLevelThree = arrPath[2];
        if (!unique.has(keyLevelOne + keyLevelTwo + keyLevelThree)) {
          const id = this.obj[keyLevelOne][keyLevelTwo][keyLevelThree].id;
          unique.add(keyLevelOne + keyLevelTwo + keyLevelThree);
          list[3].push({
            id,
            name: keyLevelThree,
            path: [keyLevelOne, keyLevelTwo, keyLevelThree],
          });
        }
      });

      // 4
      docs.forEach((arrPath) => {
        const keyLevelOne = arrPath[0];
        const keyLevelTwo = arrPath[1];
        const keyLevelThree = arrPath[2];
        const keyLevelFour = arrPath[3];

        const id = this.obj[keyLevelOne][keyLevelTwo][keyLevelThree][keyLevelFour].id;
        list[4].push({
          id,
          name: keyLevelFour,
        });
      });

      return list;
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

      /*  сохраняем значения для сравнения
       *  и изменения индекса в случае изменения
       */
      const arr0 = docs[0];
      levels[1].name = arr0[0];
      levels[2].name = arr0[1];
      levels[3].name = arr0[2];
      levels[4].name = arr0[3];

      docs.forEach((arrPath, str) => {
        let areasLine = '';

        let isLevel_2_Changed = false; // Тип документации

        for (let idx = 1; idx <= 4; idx++) {
          // увеличиваем индекс (levels[idx].index++) при смене наименования
          if (levels[idx].name !== arrPath[idx - 1]) {
            levels[idx].index++;
            levels[idx].name = arrPath[idx - 1];

            if (idx === 2) {
              areas.push(this.getLine(levels.line.index));
              levels.line.index++;
              isLevel_2_Changed = true; // Тип документации
              levels[3].index++;
            }

            if (idx === 3) {
              if (isLevel_2_Changed) {
                levels[3].index--;
              }
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

      console.table(docs);
      console.table(areas);
      return `grid-template-areas: ${areasText};`;
    },
  },

  methods: {
    mapName(arr) {
      return Array.from(new Set(arr.map((el) => el.name)));
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
      const lineClass = this.getLevelClass('line');
      return `doc-class-1 ${lineClass}-${idx}-1 ${lineClass}-${idx}-2 ${lineClass}-${idx}-3 ${lineClass}-${idx}-4`;
    },

    isLastElement(idx, arr) {
      return idx + 1 === arr.length;
    },

    editLanguage(item) {
      alert(item.path + '\nЯзык');
    },

    editVid(item) {
      alert(item.path + '\nВид документации');
    },

    editType(item) {
      alert(item.path + '\nТип документации');
    },

    deleteClass(item) {
      alert(item.id + '\nУдаление класса документации');
    },

    editDocuments(item) {
      alert(item.path + '\nДокументы');
    },
  },
};
</script>

<style>
.header {
  background: #d4dbec;
}

.grid-container {
  /* border: 1px solid #d4dbec; */
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
  background-size: contain;

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
  background-size: contain;
}

.doc-amount {
  display: flex;
  justify-content: space-between;
  border-right: 1px solid #d4dbec;
}

.doc-class {
  border: 1px solid #d4dbec;
  border-top: none;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 5px;
}

.line {
  border-bottom: 1px solid #d4dbec;
  border-right: 1px solid #d4dbec;
}

.doc-type,
.doc-lang,
.doc-vid {
  border-right: 1px solid #d4dbec;
  display: flex;
  justify-content: space-between;
}

.doc-type .trash-btn {
  height: 25px;
  width: 25px;
}

.doc-lang .trash-btn {
  height: 25px;
  width: 25px;
}

.doc-vid .trash-btn {
  height: 25px;
  width: 25px;
}
</style>
