<template>
  <li class="news-item">
    <span class="score">{{ item.score }}</span>
    <span class="title">
      <template v-if="item.url">
        <a :href="item.url" target="_blank" rel="noopener">{{ item.title }}</a>
        <span class="host"> ({{ item.url | host }})</span>
      </template>
      <template v-else>
        <router-link :to="'/item/' + item.id">{{ item.title }}</router-link>
      </template>
    </span>
    <br>
    <span class="meta">
      <span v-if="item.type !== 'job'" class="by">
        by <router-link :to="'/user/' + item.by">{{ item.by }}</router-link>
      </span>
      <span class="time">
        {{ item.time | timeAgo }} ago
      </span>
      <span v-if="item.type !== 'job'" class="comments-link">
        | <router-link :to="'/item/' + item.id">{{ item.descendants }} comments</router-link>
      </span>
    </span>
    <span class="label" v-if="item.type !== 'story'">{{ item.type }}</span>
  </li>
</template>

<script>
import { timeAgo } from "../util/filters";

export default {
  name: "news-item",
  props: ["item"],
  // http://ssr.vuejs.org/en/caching.html#component-level-caching
  serverCacheKey: ({ item: { id, __lastUpdated, time } }) => {
    return `${id}::${__lastUpdated}::${timeAgo(time)}`;
  }
};
</script>

<style lang="stylus">
.news-item {
  background-color: var(--newsItemBg, #fff);
  padding: 20px 30px 20px 80px;
  border-bottom: 1px solid var(--newsItemBorder, #eee);
  position: relative;
  line-height: var(--lineHeight, 20px);

  .score {
    color: var(--scoreColor, #ff6600);
    font-size: 1.1em;
    font-weight: 700;
    position: absolute;
    top: 50%;
    left: 0;
    width: 80px;
    text-align: center;
    margin-top: -10px;
  }

  .meta, .host {
    font-size: 0.85em;
    color: var(--metaColor, #828282);

    a {
      color: var(--metaColor, #828282);
      text-decoration: underline;

      &:hover {
        --metaColor: #ff6600;
      }
    }
  }
}
</style>
