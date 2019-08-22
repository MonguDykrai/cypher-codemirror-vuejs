<template>
  <div>
    <button @click="parseContent">Manually parse content</button>
    <div class="Codemirror-Container" ref="input" />
  </div>
</template>

<script>
import { createCypherEditor, parse } from "cypher-codemirror";
// console.log({ createCypherEditor, parse });

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
  props: ["theme", "settings", "schema"],
  data() {
    const Settings = this.settings;
    const Schema = this.schema;
    Settings.theme = this.theme;

    return {
      Settings,
      Schema,
      input: null,
      editorSupport: null,
      editor: null
    };
  },
  mounted() {
    // console.log(this.theme);
    // console.log(this.settings);
    // console.log(this.schema);

    this.input = this.$refs.input;

    console.log(this.Settings);
    console.log(this.Schema);
    console.log(this.input);

    const { editor, editorSupport } = createCypherEditor(
      this.input,
      this.Settings
    );

    console.log({ editor, editorSupport });

    this.editor = editor;
  },
  methods: {
    parseContent() {
      console.log(`parseContent`);
    }
  }
};
</script>