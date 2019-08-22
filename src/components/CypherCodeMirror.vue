<template>
  <div>
    <button @click="parseContent">Manually parse content</button>
    <div class="Codemirror-Container" ref="input" />
  </div>
</template>

<script>
import { createCypherEditor, parse } from "cypher-codemirror";
// console.log({ createCypherEditor, parse });

function triggerAutocompletion(cm /* codemirror instance */, changed) {
  console.log(cm);

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
  props: {
    props: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      settings: {
        ...this.props.settings,
        theme: this.props.theme
      },
      schema: this.props.schema,
      input: null,
      editorSupport: null,
      editor: null
    };
  },
  mounted() {
    this.input = this.$refs.input;

    const { editor, editorSupport } = createCypherEditor(
      this.input,
      this.settings
    );

    this.editor = editor;
    this.editor.on("change", triggerAutocompletion);
    this.editorSupport = editorSupport;
  },
  methods: {
    parseContent() {
      console.log(`parseContent`);
    }
  }
};
</script>