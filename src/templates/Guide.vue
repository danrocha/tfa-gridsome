<template>
  <Layout>
    <article>
    <!-- COVER IMAGE -->
    <div id="image" class="px-4">
      <div class="flex items-center justify-center w-full max-h-full mb-8 border-2 border-black image-cover guide-cover"
      :style="`background-image: url('${$page.guide.coverImage.src}')`">
        <span class="max-w-xl p-4 mx-auto text-3xl font-bold leading-none tracking-wide uppercase stripe-shadow-white">
            {{$page.guide.city}}
        </span>
      </div>
    </div>
    <header id="header" class="mb-8">
      <div class="max-w-xl px-4 mx-auto">
        <h1 class="mb-2 text-3xl font-bold leading-tight">{{ $page.guide.title }}</h1>
        <!-- <hr class="mb-4 border-b-2 border-black"/> -->
        <p class="text-xl ">{{ $page.guide.excerpt }}</p>
      </div>
    </header>
    <section id="navigation-top" v-if="$page.guide.series && $page.guide.chapter > 0" class="max-w-xl px-4 mx-auto content">
      <series-navigation
        :total-chapters="$page.guide.series.belongsTo.totalCount"
        :current-chapter="$page.guide.chapter"
        :chapters="$page.guide.series.belongsTo.edges.map(edge => edge.node)"/>
    </section>
    <section  id="content" class="max-w-xl px-4 mx-auto mb-12 content">
      <vue-remark-content class="flow"/>
    </section>
    <section  id="navigation-bottom"  v-if="$page.guide.series && $page.guide.chapter > 0" class="max-w-xl px-4 mx-auto content">
      <series-navigation
        :total-chapters="$page.guide.series.belongsTo.totalCount"
        :current-chapter="$page.guide.chapter"
        :chapters="$page.guide.series.belongsTo.edges.map(edge => edge.node)"/>
    </section>

  </article>
  </Layout>
</template>

<page-query>
query Guide ($id: ID!) {
  guide(id: $id) {
    title
    excerpt
    city
    chapter
    date
    series {
      id
      belongsTo {
        totalCount
        edges {
          node {
            ...on Guide {
              id
              title
              path
              chapter
            }
          }
        }
      }
    }
    coverImage (width: 1920, height: 800, quality: 90)
  }
}
</page-query>

<script>

export default {
  metaInfo() {
    return {
      title: this.$page.guide.title,
      meta: [
        {  name: "description", content: this.$page.guide.excerpt },
        {  name: "twitter:description", content: this.$page.guide.excerpt },
        { name: "twitter:title", content: this.$page.guide.title },
        { name: "twitter:image", content: `${process.env.GRIDSOME_BASE_URL}${this.$page.guide.coverImage.src}` },
        { name: "twitter:card", content: 'summary_large_image' },
        { property: "og:image", content: `${process.env.GRIDSOME_BASE_URL}${this.$page.guide.coverImage.src}` },
        { property: "og:image:secure_url", content: `${process.env.GRIDSOME_BASE_URL}${this.$page.guide.coverImage.src}` },
        { name: "og:type", content: "article" },
        { name: "og:title", content: this.$page.guide.title },
        {  name: "og:description", content: this.$page.guide.excerpt },
        { name: "article:published_time", content: this.$page.guide.date},
      ],
    }
  }
}
</script>

<style>
/* .grid {
  display: grid;
  grid-template-columns: 1fr;
  grid-gap: 1rem;
}
#image {
  grid-area: image;
}
#header {
  grid-area: header;
}
#content {
  grid-area: content;
}
#navigation-top {
  grid-area: navigation-top;
}
#navigation-bottom {
  grid-area: navigation-bottom;
}
@screen lg {
  .grid {
    grid-template-columns: repeat(4, 1fr);
    grid-template-areas:
      "image image header header"
      "image image content content"
      " . navigation-top content content"
  }
} */



.content h2, h3 {
  @apply font-bold;
}
.content h2 {
  @apply text-2xl leading-none p-2 inline-block;
}
.content h3 {
  @apply text-lg;
}
.content  ul {
  list-style-type: square;
}
/* .content  ul > li {
  @apply mb-2;
} */
.guide-cover {
  height: 60vh;
}

.content #buildings + ul, .content nav > ul {
  @apply list-none;
}
/* .content #buildings + ul > li, .content nav > p, .content nav > ul > li {
  @apply mb-1;
} */
</style>
