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
  // console.log(cm);

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

    this.editorSupport.on("updated", () => {
      console.log(
        "UPDATED - this.editorSupport.version: ",
        this.editorSupport.version
      );

      console.table(
        this.editorSupport.queriesAndCommands.map(stmt => stmt.getText())
      );
    });

    this.editorSupport.on("update", () => {
      console.log("UPDATE - this.editor.version: ", this.editor.version);
      console.log(
        "UPDATE - this.editorSupport.version: ",
        this.editorSupport.version
      );
      this.editorSupport
        .ensureVersion(this.editor.version)
        .then(() => {
          console.log("ENSURE OK - this.editor.version: ", this.editor.version);
          console.log(
            "ENSURE OK - this.editorSupport.version: ",
            this.editorSupport.version
          );
        })
        .catch(() => {
          console.error("Version not found");
          console.log(
            "ENSURE ERROR - this.editor.version: ",
            this.editor.version
          );
          console.log(
            "ENSURE ERROR - this.editorSupport.version: ",
            this.editorSupport.version
          );
        });
    });

    this.editorSupport.setSchema(this.schema);
  },
  methods: {
    parseContent() {
      console.log(`parseContent`);
    }
  }
};
</script>