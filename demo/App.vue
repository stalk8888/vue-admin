<template>
  <Admin :authProvider="authProvider">
    <Resource name="articles" :resourceIdName="resourceIdName" :userPermissionsField="userPermissionsField" :apiUrl="articlesApiUrl" :redirect="articlesRedirect">
      <View slot="list"   :component="ListArticles" :permissions="['admin']" />
      <View slot="show"   :component="ShowArticles" :permissions="['admin']" />
      <View slot="create" :component="CreateArticles" :permissions="['admin']" />
      <View slot="edit"   :component="EditArticles" :isPublic="true" />
    </Resource>
    <Resource name="magazines" :resourceIdName="resourceIdName" :apiUrl="magazinesApiUrl" :redirect="magazinesRedirect">
      <View slot="list"   :component="ListMagazines" :isPublic="true" />
      <View slot="show"   :component="ShowMagazines" :isPublic="true" />
      <View slot="create" :component="CreateMagazines" :isPublic="true" />
      <View slot="edit"   :component="EditMagazines" :isPublic="true" />
    </Resource>
  </Admin>
</template>

<script>

import Admin from '@components/Admin'
import Resource from '@components/Resource'

import ListArticles from './components/articles/ListArticles'
import ShowArticles from './components/articles/ShowArticles'
import CreateArticles from './components/articles/CreateArticles'
import EditArticles from './components/articles/EditArticles'

import ListMagazines from './components/magazines/ListMagazines'
import ShowMagazines from './components/magazines/ShowMagazines'
import CreateMagazines from './components/magazines/CreateMagazines'
import EditMagazines from './components/magazines/EditMagazines'

import createAxiosAdapter from './va-auth-adapter/axios.adapter'
import axios from 'axios'

const authFields = { username: 'username', password: 'password' }
const authUrl = 'http://localhost:8888/api/auth'
const client = axios
const storageKey = 'token'
const userField = 'user'

const authProvider = createAxiosAdapter(client, {
  authFields,
  authUrl,
  storageKey,
  userField,
})

// Articles Views as Array

const articlesList = [
  {
    'label': 'id',
    'type': 'TextField',
    'tag': 'h2'
  },
  {
    'label':'title',
    'type':'TextField',
    'tag':'h2'
  },
  'content'
]

const articlesShow = [
  'id',
  {
    'label':'title',
    'type': 'TextField',
    'tag': 'h1'
  },
  'content'
]

const fieldsArticleCreate =[
  {
    'label':'title',
    'type':'Input',
    'placeHolder': 'title'
  },
  'content'
]

const fieldsArticleEdit = fieldsArticleCreate

const articlesRedirect = {
  views: {
    create: 'list',
    edit: 'show'
  }
}

const magazinesRedirect = articlesRedirect

// The name of the id attribute
const resourceIdName = 'id'

// The name of the permissions field
const userPermissionsField = 'permissions'

// Use case of a parsed response using feathers
// This has to be done because every server client returns different responses. - sgobotta
//
// const parseFeathersResponses = {
//   parseList: (response) => {
//     const { data } = response
//     return Object.assign({}, response, {
//       data: data.data // Vue Admin expects a 'data' object with an array of objects
//     })
//   },
//   parseSingle: (response) => {
//     const { data } = response
//     return Object.assign({}, response, {
//       data // Vue Admin expects a 'data' object as response
//     })
//   }
// }

const articlesApiUrl = 'http://localhost:8888/api/'
const magazinesApiUrl = 'http://localhost:8888/api/'

export default {
  name: 'App',
  components: {
    Admin: Admin,
    Resource: Resource
  },
  data() {
    return {
      authProvider,
      resourceIdName,
      userPermissionsField,
      // Articles Views as Array
      articlesApiUrl,
      magazinesApiUrl,
      articlesList,
      articlesShow,
      fieldsArticleCreate,
      fieldsArticleEdit,
      articlesRedirect,
      // Articles Views as Components
      ListArticles,
      ShowArticles,
      CreateArticles,
      EditArticles,
      // Magazines Views as Custom Components
      ListMagazines,
      ShowMagazines,
      CreateMagazines,
      EditMagazines,
      magazinesRedirect

      // #23 - To use a feathers server just add the parseResponses attribute
      // below, pass ':parseResponses='parseResponses' to Resource in the
      // template and update the resourceIdName to '_id'. - sgobotta
      // parseResponses: parseFeathersResponses
    }
  }
}
</script>
