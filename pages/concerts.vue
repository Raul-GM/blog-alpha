<template>
  <div>
    <section class="main-container">
      <BlogsList :blogs="blogs" />
    </section>
  </div>
</template>
<script>
import blogsEs from '~/content/es/blogsEs.js'
import blogsEn from '~/content/en/blogsEn.js'
import BlogsList from '~/components/sections/BlogsList'

export default {
  components: { BlogsList },
  head() {
    return {
      title: 'concerts',
      meta: [
        {
          hid: 'title',
          name: 'title',
          content: 'Concerts'
        },
        {
          hid: 'description',
          name: 'description',
          description: 'Concerts section. Reviews of all the concerts'
        }
      ]
    }
  },
  async asyncData({ app }) {
    const locale = app.i18n.locale
    const blogs = locale === 'es' ? blogsEs : blogsEn
    async function asyncImport(blogName) {
      const post = await import(`~/content/${locale}/blog/${blogName}.md`)
      return post.attributes
    }
    const allPosts = await Promise.all(
      blogs.map((blog) => asyncImport(blog))
    ).then((res) => {
      return {
        blogs: res
      }
    })
    return {
      blogs: allPosts.blogs.filter((post) => post.categories.includes('gig'))
    }
  }
}
</script>
<style>
.main-container {
  align-items: center;
  display: flex;
  flex-direction: column;
  padding: 5em 1em;
}
</style>
