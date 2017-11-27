<template>
    <div id="app">
    <header class="navbar">
      <span class="navbar-text"> Image Upload with Vue, Axios
      </span>
    </header>

    <div class="container">
      <div style="display:flex; flex-direction: column;">
          <img :src="imageSrc" class="image">
          <span class="sub-text"><a :href="url_s" target="_blank">Small</a> | <a :href="url_m" target="_blank">Medium</a> | <a :href="url_l" target="_blank">Large</a></span>
          <span class="btn btn-default btn-file">
              Select And Upload <input @change="uploadImage" type="file" name="image" accept="image/*">
          </span>
        </div>
    </div>
  </div>
</template>

<script>
  import * as axios from 'axios';

  const BASE_URL = 'http://localhost:3000';

  export default {
    name: 'app',
    data () {
      return {
        imageSrc: '',
        url_s: '',
        url_m: '',
        url_l: ''
      }
    },
    methods: {
      uploadImage(e) {
        var files = e.target.files;
        if(!files[0]) {
          return;
        }
        var data = new FormData();
        data.append('avatar', files[0]);

        var reader = new FileReader();
        reader.onload = (e) => {
          this.imageSrc = e.target.result;
        };

        let thisObj = this;
        axios.post(`${BASE_URL}/upload`, data, {headers: { 'Content-Type': 'multipart/form-data' }})
        .then((response) => {
          console.log('Uploaded.');
          thisObj.url_s = `${BASE_URL}/images/${response.data.image_s_id}`
          thisObj.url_m = `${BASE_URL}/images/${response.data.image_m_id}`
          thisObj.url_l = `${BASE_URL}/images/${response.data.image_l_id}`
          reader.readAsDataURL(files[0]);
        })
        .catch((error) => {
          console.log(error) // catch your error
        });
      },
      test() {
        console.log('test')
      }
    }
  }
</script>

<style>
  body {
    margin: 0;
  }
  .image {
    width: 100%;
    height: 100%;
    margin-bottom:40px;
  }
  .container {
    margin: 0 auto;
    width: 600px;
  }
  .navbar {
    width: 100%;
    height: 65px;
    display: flex;
    background: #f2a832;
    align-items: center;
    padding-left: 35px;
    margin-bottom:50px;
  }
  .navbar-text {
    color: #ffffff;
    font-weight: bold;
    font-size: 22px;
  }
  .sub-text {
    color: #2929ce;
    font-weight: bold;
    font-size: 14px;
    padding-bottom: 10px;
  }
  .btn {
    cursor: pointer;
    display: inline-block;
    font-weight: 700;
    text-align: center;
    white-space: nowrap;
    font-size: 17px;
    padding: 12px 25px;
    min-width: 150px;
    position: relative;
  }
  .btn-default {
    color: #fff;
    background: #f2a832;
    border-color:#f2a832;
  }
  .btn-file input[type=file] {
    position: absolute;
    top: 0;
    right: 0;
    min-width: 100%;
    min-height: 100%;
    font-size: 100px;
    text-align: right;
    filter: alpha(opacity=0);
    opacity: 0;
    outline: none;
    background: white;
    cursor: inherit;
  }
</style>