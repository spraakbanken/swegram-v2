<template>
  <div>
    <el-row>
      <div class="section-name">
        <h3>{{ textName.split('.').slice(0, textName.split('.').length-1).join('.') }}</h3>
      </div>
    </el-row>
    <el-row
      v-show="showMetadata"
    >
      <div class="section-metadata">
        <h5>Metadata:</h5>
        <span>
          <el-tag
            v-for="(label, i) in metaData"
            :key="i"
            type="info"
          >
            {{ label[0] }} : {{ label[1] }}
          </el-tag>
        </span>
      </div>
    </el-row>
    <el-row class="section-body">
      <el-collapse>
        <el-collapse-item
          v-for="(snippet, i) in snippets"
          :key="i"
        >
          <template slot="title">
            <div class="snippet-title">
              <span>{{ snippet.tokens[0].text_id }} </span>
              <!-- <span>({{ i + 1 + pageSize * (pageIndex - 1) }}.)</span> -->
              <span
                v-for="(t, j) in snippet.tokens"
                :key="j"
                :class="t.highlight"
              >{{ t.form }} </span>
            </div>
          </template>
          <div
            class="snippet-sentence-tags"
          >
            <el-popover
              v-for="(token, k) in snippet.tokens"
              :key="k"
              placement="top"
              trigger="click"
            >
              <h4>{{ $t('visualize.markedToken') }}</h4>
              {{ $t('visualize.form') }}: <span>{{ token.form }}</span><br>
              {{ $t('visualize.norm') }}: <span>{{ token.norm }}</span><br>
              {{ $t('visualize.lemma') }}: <span>{{ token.lemma }}</span><br>
              {{ $t('visualize.upos') }}: <span>{{ token.upos }}</span><br>
              {{ $t('visualize.xpos') }}: <span>{{ token.xpos }}</span><br>
              {{ $t('visualize.feats') }}: <span>{{ token.feats }}</span><br>
              {{ $t('visualize.ufeats') }}: <span>{{ token.ufeats }}</span><br>
              {{ $t('visualize.head') }}: <span>{{ token.head }}</span><br>
              {{ $t('visualize.deprel') }}: <span>{{ token.deprel }}</span><br>
              {{ $t('visualize.deps') }}: <span>{{ token.deps }}</span><br>
              {{ $t('visualize.misc') }}: <span>{{ token.misc }}</span><br>
              {{ $t('visualize.dependencyLength') }}: <span>{{ token.dep_length }}</span><br>
              {{ $t('visualize.stig') }}: <span>{{ token.path }}</span><br>
              <el-tag
                slot="reference"
              >
                {{ token.form }}
                <el-popover
                  v-if="token.compound_originals"
                  placement="top"
                  trigger="click"
                >
                  <h4>{{ $t('visualize.markedToken') }}</h4>
                  {{ $t('visualize.form') }}: <span>{{ token.form }}</span><br>
                  {{ $t('visualize.norm') }}: <span>{{ token.norm }}</span><br>
                  {{ $t('visualize.lemma') }}: <span>{{ token.lemma }}</span><br>
                  {{ $t('visualize.upos') }}: <span>{{ token.upos }}</span><br>
                  {{ $t('visualize.xpos') }}: <span>{{ token.xpos }}</span><br>
                  {{ $t('visualize.feats') }}: <span>{{ token.feats }}</span><br>
                  {{ $t('visualize.ufeats') }}: <span>{{ token.ufeats }}</span><br>
                  {{ $t('visualize.head') }}: <span>{{ token.head }}</span><br>
                  {{ $t('visualize.deprel') }}: <span>{{ token.deprel }}</span><br>
                  {{ $t('visualize.deps') }}: <span>{{ token.deps }}</span><br>
                  {{ $t('visualize.misc') }}: <span>{{ token.misc }}</span><br>
                  {{ $t('visualize.dependencyLength') }}: <span>{{ token.dep_length }}</span><br>
                  {{ $t('visualize.stig') }}: <span>{{ token.path }}</span><br>
                  <b slot="reference">{{ token.norm }}</b>
                </el-popover>
              </el-tag>
            </el-popover>
          </div>
          <div
            v-if="parsed"
            class="snippet-data-container"
          >
            <displacy-image
              class="displacy-container"
              :sentence="snippet"
            />
          </div>
        </el-collapse-item>
      </el-collapse>
    </el-row>
  </div>
</template>

<script>
import DisplacyImage from './DisplacyImage.vue';

export default {
  components: {
    DisplacyImage,
  },
  props: {
    snippets: {
      type: Array,
      default: () => [],
      required: false,
    },
    metaData: {
      type: Array,
      default: () => [],
      required: false,
    },
    textName: {
      type: String,
      default: null,
      required: false,
    },
    pageSize: {
      type: Number,
      default: null,
      required: false,
    },
    pageIndex: {
      type: Number,
      default: null,
      required: false,
    },
    parsed: {
      type: Boolean,
      default: false,
      required: false,
    },
  },
  computed: {
    showMetadata() {
      return this.$props.metaData.length > 0;
    },
  },
};
</script>

<style scoped>

.highlight {
  color: red;
  font-weight: bold;
}

.section-name {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.section-name h3 {
  margin-right: 10px;
}

.section-name {
  flex-flow: wrap;
}

.section-body {
  margin: 0 0 0 30px;
}

.snippet-title {
  height: 48px;
  width: 100%;
  overflow: hidden;
}

.snippet-data-container {
  display: flex;
  margin: 4px;
  justify-content: center;
}

.displacy-container {
  width: 80%;
  max-height: 400px;
}

.el-tag {
  margin: 4px;
}

</style>
