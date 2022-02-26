<template src="./Home.html"></template>

<script lang="ts">
export default {
  name: "Home",
  data() {
    return {
      lists: [{ people: [] }, { people: [] }],
    };
  },
  methods: {
    styling(perso) {
      return { background: perso.giveTo || "white" };
    },
    assigment() {
      let [{ people: listA }, { people: listB }] = this.$data.lists;

      this.lists = [
        { people: this.assigmentList([...listA], [...listB]) },
        { people: this.assigmentList([...listB], [...listA]) },
      ];
    },
    assigmentList(listA, listB) {
      listB = [...listB.map(({ name }) => ({ name, giveTo: null }))];

      return listA.map((people) => {
        listB = listB.filter((person) => !person.giveTo);

        if (listB.length === 0) {
          return people;
        }

        const selected = Math.floor(Math.random() * Math.floor(listB.length));

        const name = listB[selected].name;

        listB[selected].giveTo = name;

        people.giveTo = name;

        return people;
      });
    },
    remove(list, index) {
      list.splice(index, 1);
    },
    submit(list, event) {
      list.people.push({ name: event.target.value, giveTo: null });

      event.target.value = "";
    },
    listAreEqual() {
      const [listA, listB] = this.$data.lists;

      return (
        listA.people.length !== listB.people.length || listA.people.length === 0
      );
    },
    withSeparator(index) {
      return {
        separator: index + 1 !== this.$data.lists.length,
      };
    },
  },
};
</script>
<style scoped src="./Home.css"></style>
