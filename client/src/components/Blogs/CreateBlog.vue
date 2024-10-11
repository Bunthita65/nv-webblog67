<template>
  <div>
    <h1>Create Art Work</h1>
    <form v-on:submit.prevent="createBlog">
      <p>
        Name:
        <input type="text" v-model="blog.title" />
      </p>

      <transition name="fade">
        <div class="thumbnail-pic" v-if="blog.thumbnail !== 'null'">
          <img :src="BASE_URL + blog.thumbnail" alt="thumbnail" />
        </div>
      </transition>

      <div class="dropbox">
        <input
          type="file"
          multiple
          :name="uploadFieldName"
          :disabled="isSaving"
          @change="filesChange($event.target.name, $event.target.files)"
          accept="image/*"
          class="input-file"
        />
        <p v-if="isInitial">Drag your file(s) here to begin<br />or click to browse</p>
        <p v-if="isSaving">Uploading {{ fileCount }} files...</p>
        <p v-if="isSuccess">Upload Successful.</p>
      </div>

      <div>
        <transition-group tag="ul" class="pictures">
          <li v-for="picture in pictures" v-bind:key="picture.id">
            <img style="margin-bottom: 5px" :src="BASE_URL + picture.name" alt="picture image" />
            <br />
            <button v-on:click.prevent="useThumbnail(picture.name)">Thumbnail</button>
            <button v-on:click.prevent="delFile(picture)">Delete</button>
          </li>
        </transition-group>
        <div class="clearfix"></div>
      </div>

      <p><strong>Details:</strong></p>
      <vue-ckeditor v-model.lazy="blog.content" :config="config" @blur="onBlur($event)" @focus="onFocus($event)" />

      <p>
        Category:
        <input type="text" v-model="blog.category" />
      </p>
      <p>
        Status:
        <input type="text" v-model="blog.status" />
      </p>
      <p>
        <button type="submit">Create Blog</button>
      </p>
    </form>
  </div>
</template>
