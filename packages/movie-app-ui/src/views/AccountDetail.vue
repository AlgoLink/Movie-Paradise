<template>
  <v-page
    :title="$t('account')"
  >
    <v-list>
      <v-list-item
        @click="goEdit"
      >
        <v-list-item-avatar color="grey">
          <img
            v-if="userStore.avatar"
            :src="userStore.avatar"
            :alt="userStore.name + ' avatar'"
          >
        </v-list-item-avatar>
        <v-list-item-content>
          <v-list-item-title class="headline">
            {{ userStore.name }}
          </v-list-item-title>
          <v-list-item-subtitle>{{ userStore.desc }}</v-list-item-subtitle>
        </v-list-item-content>
        <v-list-item-action>
          <v-btn icon>
            <v-icon>mdi-chevron-right</v-icon>
          </v-btn>
        </v-list-item-action>
      </v-list-item>

      <v-list-item
        v-for="(list,index) in lists"
        :key="index"
        @click="switchMethod(list.method)"
      >
        <v-list-item-icon>
          <v-icon>{{ list.icon }}</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{ list.text }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <account-detail-delete
        @logOut="logOut"
      />
    </v-list>
  </v-page>
</template>

<script>
import VPage from '../components/global/VPage'
import AccountDetailDelete from '../components/AccountDetailDelete'
import storeMap from '../mixins/storeMap'
import Message from '../utils/message'
import appMixin from '../mixins/appMixin'

export default {
  name: 'AccountDetail',
  components: {
    'v-page': VPage,
    'account-detail-delete': AccountDetailDelete
  },
  mixins: [storeMap, appMixin],
  data () {
    return {
      lists: [
        {
          text: this.$t('myComments'),
          icon: 'mdi-comment-text-multiple ',
          method: 'goComment'
        },
        {
          text: this.$t('myList'),
          icon: 'mdi-view-list',
          method: 'goList'
        }, {
          text: this.$t('myLike'),
          icon: 'mdi-heart',
          method: 'goLike'
        }, {
          text: this.$t('watched'),
          icon: 'mdi-movie-filter',
          method: 'goWatched'
        }, {
          text: this.$t('logout'),
          icon: 'mdi-logout',
          method: 'logOut'
        }
      ]
    }
  },
  methods: {
    switchMethod (method) {
      this[method]()
    },
    logOut () {
      this.clearLoginData()
      Message.success()
      this.$router.back()
    },
    goEdit () {
      this.$router.push({ path: '/account/edit' })
    },
    // todo: those list should have a edit and delete function. For mobile, swipe, for desktop show button
    goComment () {
      this.$router.push({ path: '/account/comments' })
    },
    goList () {
      this.$router.push({ path: '/account/movies', query: { title: this.$t('myList') } })
    },
    goLike () {
      this.$router.push({ path: '/account/movies', query: { title: this.$t('myLike') } })
    },
    goWatched () {
      this.$router.push({ path: '/account/movies', query: { title: this.$t('watched') } })
    }
  }
}
</script>

<style scoped>

</style>
