<style lang="scss">
html,
body {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
}

body {
  background-color: lavender;
  font-family: "Merriweather", Georgia, "Times New Roman", Times, serif;
}

.container {
  width: 100%;
  height: 100%;
  margin: 0 auto;
  font-size: 16px;
}

.columns {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 100%;
  justify-content: flex-start;
  align-items: stretch;
  align-content: stretch;
}

.columns > div {
  width: 100%;
  height: 100%;
  flex: 1;
  overflow: auto;
}

.schema {
  padding: 10px;
  font-size: 10pt;
}

.CodeMirror {
  height: 100vh !important;
}
</style>


<template>
  <div id="app">
    <CypherCodeMirror :props="{ theme, settings: codeMirrorSettings, schema: neo4jSchema }" />

    <!-- <div class="columns">
      <div>
        <CypherCodeMirror :props="{ theme, settings: codeMirrorSettings, schema: neo4jSchema }" />
      </div>
      <div>
        <div class="schema">
          <button @click="lightTheme">Light theme</button>
          <button @click="darkTheme">Dark theme</button>
          <pre>{{ JSON.stringify(neo4jSchema, null, 2) }}</pre>
        </div>
      </div>
    </div> -->
  </div>
</template>

<script>
import "codemirror/lib/codemirror.css";
import "codemirror/addon/lint/lint";
import "codemirror/addon/lint/lint.css";
import "codemirror/addon/hint/show-hint";
import "codemirror/addon/edit/closebrackets";
import "cypher-codemirror/dist/cypher-codemirror-all.css";
import CypherCodeMirror from "@/components/CypherCodeMirror.vue";
import { neo4jSchema, codeMirrorSettings } from "@/assets/common";

export default {
  name: "App",
  data() {
    return {
      theme: "cypher cypher-dark",
      codeMirrorSettings,
      neo4jSchema
    };
  },
  components: {
    CypherCodeMirror
  },
  methods: {
    lightTheme() {
      this.theme = "cypher";
    },
    darkTheme() {
      this.theme = "cypher cypher-dark";
    }
  }
};
</script>