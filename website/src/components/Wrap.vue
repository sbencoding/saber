<template>
  <div class="wrap">
    <Header />
    <Toc
      v-if="page.markdownHeadings && page.markdownHeadings.length > 0"
      :headings="page.markdownHeadings"
    />
    <div class="main">
      <div class="container flex">
        <Leftbar :items="$themeConfig.sidebarMenu" :hide="!showLeftbar">
          <template #content>
            <slot name="sidebar" />
          </template>
        </Leftbar>
        <div class="page" :class="{'no-leftbar': !showLeftbar, 'no-rightbar': !showRightbar}">
          <div class="content" :class="{fullwidth: contentFullWidth}">
            <slot name="default" />
            <div class="edit-info" v-if="showEditInfo">
              <span class="last-edited">Last Edited on {{ updatedDate }}</span>
              <a class="edit-link" target="_blank" :href="editLink">Edit This Page on GitHub</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import format from 'date-fns/format'
import Header from './Header.vue'
import Leftbar from './Leftbar.vue'
import Toc from './Toc.vue'

export default {
  components: {
    Header,
    Leftbar,
    Toc
  },

  props: {
    page: {
      type: Object,
      required: true
    },
    showLeftbar: {
      type: Boolean,
      default: true
    },
    showRightbar: {
      type: Boolean,
      default: true
    },
    showEditInfo: {
      type: Boolean,
      default: true
    },
    contentFullWidth: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    updatedDate() {
      return format(this.page.updatedAt, 'MMMM DD, YYYY')
    },

    editLink() {
      const { slug, type } = this.page
      return `https://github.com/egoist/saber/blob/master/website/pages/${
        type === 'post' ? '_posts/' : ''
      }${slug}.md`
    }
  }
}
</script>

<style scoped>
.page.no-leftbar {
  padding-left: 0;
}

.content.fullwidth {
  width: 100%;
}

.edit-info {
  margin-top: 20px;
  padding-top: 20px;
  color: var(--text-light-color);
  font-size: 0.8rem;
  border-top: 1px solid var(--border-color);
  display: flex;
  justify-content: space-between;
}

.rightbar {
  @media (max-width: 1024px) {
    display: none;
  }
}
</style>
