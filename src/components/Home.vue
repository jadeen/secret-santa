<template>
  <div class="home">
    <h1>Secret santa</h1>
    <div class="body">
      <div class="content">
        <div
          v-for="(list, index) in lists"
          v-bind:key="index"
          class="list"
          v-bind:class="{ separator: index + 1 !== lists.length }"
        >
          <input v-on:keyup.enter="submit(list, $event)" placeholder="name" />
          <div class="people">
            <div class="line" v-for="(person, index) in list.people" v-bind:key="index">
              <p>
                {{ person.name }}
                <template v-if="person.giveTo">
                  <span class="arrow">&#8594;</span>
                  <span>{{ person.giveTo }}</span>
                </template>
              </p>
              <i class="material-icons remove" v-on:click="remove(list.people, _index)">close</i>
            </div>
          </div>
        </div>
      </div>

      <button v-on:click="assigment" :disabled="listAreEqual()">START ASSIGNMENT</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class HomePage extends Vue {
  lists = [
    {
      people: []
    },
    {
      people: []
    }
  ];
  styling(perso: any) {
    return { background: perso.giveTo || "white" };
  }

  assigment() {
    let [{ people: listA }, { people: listB }] = this.lists;

    this.lists = [
      { people: this.assigmentList([...listA], [...listB]) },
      { people: this.assigmentList([...listB], [...listA]) }
    ];
  }

  assigmentList(listA: any, listB: any) {
    listB = [...listB.map(({ name }: any) => ({ name, giveTo: null }))];

    return listA.map((people: any) => {
      listB = listB.filter((person: any) => !person.giveTo);

      if (listB.length === 0) {
        return people;
      }

      const selected = Math.floor(Math.random() * Math.floor(listB.length));

      const name = listB[selected].name;

      listB[selected].giveTo = name;

      people.giveTo = name;

      return people;
    });
  }

  remove(list: any, index: number) {
    list.splice(index, 1);
  }

  submit(list: any, event: any) {
    list.people.push({ name: event.target.value, giveTo: null });

    event.target.value = "";
  }

  listAreEqual() {
    const [listA, listB] = this.lists;

    return (
      listA.people.length !== listB.people.length || listA.people.length === 0
    );
  }

  withSeparator(index: number) {
    return {
      separator: index + 1 !== this.lists.length
    };
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.home {
  height: inherit;
}

.content {
  display: flex;
  width: 75%;
  height: inherit;
  margin: 0 auto;
}

#app {
  height: calc(100% - 48px);
}

.body {
  height: inherit;
  display: flex;
  flex-direction: column;
  align-items: center;
}

button {
  border: none;
  background: #0a5692;
  color: white;
  height: 36px;
  padding: 0 24px;
  border-radius: 3px;
}

button:enabled {
  cursor: pointer;
}

button:disabled {
  background: lightgray;
  color: black;
}

button:hover:enabled {
  background: #0f71bf;
}

button:focus {
  outline: none;
}

input {
  border: none;
  border: 1px solid lightgrey;
  border-radius: 3px;
  padding: 10px 10px;
  font-size: 14px;
  line-height: 20px;

  background: inherit;
  width: calc(100% - 20px);
}

.line {
  display: flex;
  align-items: center;
}

body {
  margin: 0;
  height: calc(100% - 48px);
}

html {
  margin: 0;
  height: 100%;
}

.separator {
  border-right: 1px solid lightgray;
}

body {
  padding: 24px;
}

.people {
  height: calc(100% - 72px);
  overflow: auto;
  margin-top: 24px;
}

h1 {
  font-family: "Christmas";
  padding: 48px 0;
  margin: 0;
  font-size: 60px;
  letter-spacing: 10px;
  text-align: center;
}

.arrow {
  margin: 0 1em;
}

p {
  font-size: 14px;
  line-height: 20px;
  margin: 0;
  padding: 1em;
  display: flex;
  flex: 1;
  font-family: sans-serif;
}

.list {
  flex: 1px;
  margin: 24px 0;
  padding: 24px;
}

.list:not(.list:last-child) {
  border-right: 1px solid lightgray;
}

input:focus {
  outline: none;
}

.remove {
  cursor: pointer;
  color: darkred;
  font-size: 18px !important;
}

.remove:hover {
  color: #bb0000;
}
</style>
