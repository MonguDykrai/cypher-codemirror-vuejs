<template>
  <div>CypherCodeMirror</div>
</template>

<script>
import { createCypherEditor, parse } from "cypher-codemirror";
console.log({ createCypherEditor, parse });

function triggerAutocompletion(cm, changed) {
  if (changed.text.length !== 1) {
    return;
  }

  const text = changed.text[0];
  const shouldTriggerAutocompletion =
    text === "." ||
    text === ":" ||
    text === "[]" ||
    text === "()" ||
    text === "{}" ||
    text === "[" ||
    text === "(" ||
    text === "{" ||
    text === "$";
  if (shouldTriggerAutocompletion) {
    cm.execCommand("autocomplete");
  }
}

export default {
  name: "CypherCodeMirror",
  props: ["theme"],
  mounted() {
    console.log(this.theme);
  }
};
</script>