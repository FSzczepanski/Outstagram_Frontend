<template>
<div>
  <button  class="btnLogout" style="position: absolute; margin-top: 120px; margin-left:1600px" @click="postDialogClosed = !postDialogClosed"> Add new post</button>/>
  <transition name="slide-fade">
    <dialog open v-if="!postDialogClosed" >
      <img src="./assets/close.png" width="40" height="40" style="margin-left: 610px" v-on:click="postDialogClosed = true" />
      <h1 style="display: flex; justify-content: center; margin-top:20px">Add new Post</h1>
      <textarea class="myinput" style="height: 1.5rem;" v-model="title" placeholder="title"></textarea>
      <textarea class="myinput" v-model="desc" placeholder="description"></textarea>
      <textarea class="myinput" style="height: 1.5rem;" v-model="sign" placeholder="signature"></textarea>
      <form style="margin-left:220px; margin-top:20px; color: black; font-size:25px;">
        <input type="file" id="file" ref="file" v-on:change="handleFileUpload()"/>
      </form>
      <br />
      <button class="btnLogout" style="margin-left: 300px; margin-top: 15px; position: absolute;" @click="postPost(title,desc,sign)">Add </button>
    </dialog>
    </transition>

    <transition name="slide-fade">
    <dialog open v-if="!editDialogClosed" >
      <img src="./assets/close.png" width="40" height="40" style="margin-left: 610px" v-on:click="editDialogClosed = true" />
      <h1 style="display: flex; justify-content: center; margin-top:20px">Edit Post</h1>
      <textarea class="myinput" style="height: 1.5rem;" v-model="title" placeholder="title"></textarea>
      <textarea class="myinput" v-model="desc" placeholder="description"></textarea>
      <textarea class="myinput" style="height: 1.5rem;" v-model="sign" placeholder="signature"></textarea>
      
      <br />
      <button class="btnLogout" style="margin-left: 300px; margin-top: 15px; position: absolute;" @click="editPost(title,desc,sign)">Edit </button>
    </dialog>
    </transition>

    

 <div style="display: flex; justify-content: center;">
        <h2 style="margin-top: 20px; font-size: 95px;font-family: 'Brush Script MT', cursive;">Outstagram</h2>
         
    </div>
   

    <ul id="list-rendering">
         <div v-for="item in posts" v-bind:key="item" style="display: flex; justify-content: center">
            <div class="card-body newElement element" @click="editDialogClosed = !editDialogClosed, clickedPostId= item.id">
                <div style="margin-left: 130px;margin-top:40px; ">
                    <p class="card-text" style="font-size:35px" >{{item.title}}</p>
                    <img class="images"  :src="require('./../../../../../sem4/outstagram/outstagram/outStagram/wwwroot/pictures/'+item.pictureUrl)"/>
                    <p class="card-text" style="color: rgb(143, 118, 204); font-weight: bold;" >Description</p>
                    <p class="card-text" >{{item.description}}</p>
                    <p class="card-text" style="color: rgb(143, 118, 204); font-weight: bold; " >Author</p>
                    <p class="card-text">{{item.author}}</p>
                </div>
                <div style="margin-left:150px">
                    <img src="./assets/bin.png" width="60" height="60" style="margin-right: 40px; margin-top:30px;" v-on:click="deletePost(item.id)" />
                </div>
             </div>
         </div>
    </ul>
    <div>
        
    </div>
    
    
</div>
</template>

<script>

export default {
  name: 'MainPage',
  components: {

  },
  data() {
            return {
            postDialogClosed: true,
            editDialogClosed: true,
            clickedPostId: null,
            posts: null,
            picture: null,
            imgPreUrl: './assets/',
            file: ''
            }
            },
            created: function(){
                this.getPosts();
            }, methods: {
              getPosts(){
                fetch("https://localhost:44373/api/post", {
                method: 'get'
                })
                .then(response => response.json())
                .then(posts => this.posts = posts).then(console.log("this.posts"));
              },
              handleFileUpload(){
                this.file = this.$refs.file.files[0];
               },
              postPost(title,description,sign) {
                let formData = new FormData();
                formData.append('title',title);
                formData.append('description',description);
                formData.append('author',sign);
                formData.append('pictureFile', this.file);
                
                  this.postDialogClosed = true;
             
              fetch("https://localhost:44373/api/post", {
                  method: 'POST',
                  body: formData
                })
                .then(async response => {
                  const data = await response.json();
                  
                 
                  // check for error response
                  if (!response.ok) {
                    // get error message from body or default to response status
                    const error = (data && data.message) || response.status;
                    return Promise.reject(error);
                  }
                
                  await this.getPosts();
                })
                .catch(error => {
                  this.errorMessage = error;
                  console.error('There was an error!', error);
                })
              
             
              },
              deletePost(id) {
                  //alert(id)
                  fetch("https://localhost:44373/api/post/"+id, {
                  method: 'DELETE'
                })
                .then(async response => {
                  const data = await response.json();
                 
                  if (!response.ok) {
                    const error = (data && data.message) || response.status;
                    return Promise.reject(error);
                  }

                  this.getPosts();
                })
                .catch(error => {
                  this.errorMessage = error;
                  console.error('There was an error!', error);
                })
              },
              editPost(title,description,sign){
               // alert(id + todoDesc);

                const requestOptions = {
                method: "PATCH",
                headers: {
                    "Content-Type": "application/json",
                    },
                body: JSON.stringify({ title: title,description: description, author:sign })
              };
              this.editDialogClosed=true;
              fetch("https://localhost:44373/api/post/"+this.clickedPostId, requestOptions)
                .then(async response => {
                const data = await response.json();
                
                  
                
                // check for error response
                if (!response.ok) {
                  // get error message from body or default to response status
                  const error = (data && data.message) || response.status;
                  return Promise.reject(error);

                }else{
                  this.getPosts();
                }
                

              })
              .catch(error => {
                this.errorMessage = error;
                console.error('There was an error!', error);
              });
              }
              
            }
}
       
       

</script>

<style scoped>

.images{
  width: 120%;
  max-width: 900px;
  max-height: 400px;
}

dialog {
  position: absolute;
  color: rgb(255, 255, 255);
  width: 700px;
  height: 580px;
  margin-top: 150px;
  background: rgba(208, 230, 230, 0.94);
  filter: drop-shadow(0px 4px 4px rgba(0, 0, 0, 0.25));
  border: solid gray 2px;
  border-radius: 40px;
}

.newElement{
        font-size: 18px;
        width: 960px;
        height: 800px;
        display: flex;
        flex-direction: row;
        justify-content: center;
         background: rgba(235, 235, 235, 0.4);
    }

    .myinput{
  text-align: center;
  display: block;
    margin-left: auto;
    margin-right: auto;
  margin-top: 40px;
  height: 100px;
  width: 400px;
  border: 2px solid rgb(195, 166, 223);
  filter: drop-shadow(0px 4px 4px rgba(0, 0, 0, 0.25));
  background-color: none;
  font-size: 20px;
  border-radius: 4px;
  padding: 10px;
}


.slide-fade-enter-active {
  transition: all .2s ease-out;
}

.slide-fade-leave-active {
  transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(30px);
  opacity: 0;
}

.btnLogout{
  display: inline-block;
  background: rgb(143, 118, 204);
  color: rgb(238, 234, 238);
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 22px;
}
</style>