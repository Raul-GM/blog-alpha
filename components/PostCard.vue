<template>
  <nuxt-link :to="`${urlPost}`">
    <div :style="{ 'background-image': `url(${mainImage})` }" class="post-card">
      <span class="post-card__date">{{ date | formateDate }}</span>
      <div class="post-card__info">
        <h3 class="post-card__info-title">{{ title }}</h3>
        <p class="post-card__info-description">{{ description }}</p>
      </div>
    </div>
  </nuxt-link>
</template>
<script>
export default {
  props: {
    title: {
      type: String,
      default: ''
    },
    description: {
      type: String,
      default: ''
    },
    id: {
      type: String,
      default: ''
    },
    date: {
      type: String,
      default: ''
    }
  },
  computed: {
    urlPost() {
      const locale = this.$i18n.locale === 'es' ? '' : `/${this.$i18n.locale}`
      return `${locale}/blog/${this.id}`
    },
    mainImage() {
      if (!this.id) return null
      return require(`../assets/images/blog/${this.id}/_main.jpg`)
    }
  }
}
</script>
<style lang="scss">
:root {
  --card-width: 250px;
  --card-height: 300px;
}
@media (min-width: 1000px) {
  :root {
    --card-width: 290px;
    --card-height: 320px;
  }
}
.post-card {
  background-size: cover;
  background-position: center;
  border: 0;
  border-radius: 6px;
  height: var(--card-height);
  overflow: hidden;
  position: relative;
  width: var(--card-width);
  display: block;
  z-index: 5;
  box-shadow: -5px -5px 0 2px var(--skull), 5px 5px 0 2px var(--raven);
  &:hover .post-card__info-description {
    max-height: 200px;
    padding-bottom: 0.5rem;
  }
  &__date {
    background-color: var(--background-color-semitransparent);
    line-height: 1;
    padding: 0.6em;
    position: absolute;
    right: 0;
  }
  &__info {
    background-color: var(--background-color-semitransparent);
    bottom: 0;
    position: absolute;
    width: 100%;
    &-title {
      font-family: var(--font-subtitle);
      font-size: 1.1rem;
      font-weight: 600;
      padding: 0.5rem 1rem;
      text-align: center;
    }
  }
  &__info-description {
    font-size: 0.9rem;
    margin: 0 1rem;
    max-height: 0;
    padding-bottom: 0rem;
    transition: 0.3s max-height ease-in-out, 0.3s padding-bottom 0.1s;
  }
}
</style>
