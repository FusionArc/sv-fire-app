<script>
  import { FirebaseApp, User, Doc, Collection, StorageRef } from "sveltefire";

  import firebase from "firebase/app";
  import "firebase/firestore";
  import "firebase/storage";
  import "firebase/auth";
  import "firebase/performance";
  import "firebase/analytics";

  let firebaseConfig = {
    apiKey: "AIzaSyAQJQRCg5TasZH2K4YPUf0APKQRzAuj8h0",
    authDomain: "sv-fire.firebaseapp.com",
    databaseURL: "https://sv-fire.firebaseio.com",
    projectId: "sv-fire",
    storageBucket: "sv-fire.appspot.com",
    messagingSenderId: "70863762306",
    appId: "1:70863762306:web:5f55379ad6753e2b4f959b",
    measurementId: "G-7JX0K0PQRV"
  };
  
  firebase.initializeApp(firebaseConfig);
</script>

<main>
<!-- 
  {#if !firebaseConfig.projectId}
    <strong>Step 0</strong>
    Create a
    <a href="https://firebase.google.com/">Firebase Project</a>
    and paste your web config into
    <code>App.svelte</code>
    .
  {/if} -->

  <!-- 1. 🔥 Firebase App -->
  <FirebaseApp {firebase}>
    <h1>🔥 Activated</h1>
    
  <!-- 
    <p>
      <strong>Tip:</strong>
      Open the browser console for development logging.
    </p> -->  
    <!-- 2. 😀 Get the current user -->
    <User persist={sessionStorage} let:user={user} let:auth={auth} on:user >
      {user.uid}
      <button on:click={() => auth.signOut()}>Sign Out</button>
  
      <div slot="signed-out">
        <button on:click={() => auth.signInAnonymously()}>
          Sign In Anonymously
        </button>
        <button on:click={() => auth.signInWithProvider()}>
        
        

      </div>

    
    
<!--     
    <User let:user let:auth>
     User<em>{user.uid}</em>
          
     <div slot="signed-out">
       
      </div> -->
      
      <hr />
      
      <!-- 3. 📜 Get a Firestore document owned by a user -->
      <Doc path={`posts/${user.uid}`} let:data={post} let:ref={postRef} log>
        
        <h2>{post.title}</h2>
        
        <p>
          Document
          created at <em>{new Date(post.createdAt).toLocaleString()}</em>
        </p>
        
        <span slot="loading">Loading post...</span>
        <span slot="fallback">
          <button
          on:click={() => postRef.set({
            title: '📜 I like Svelte',
            createdAt: Date.now()
          })}>
            Create Document
          </button>
        </span>
        
        <!-- 4. 💬 Get all the comments in its subcollection -->
        
        <h3>Comments</h3>
        <Collection
        path={postRef.collection('comments')}
        query={ref => ref.orderBy('createdAt')}
        let:data={comments}
        let:ref={commentsRef}
        log>
        
        {#if !comments.length}
        No comments yet...
        {/if}
        
        {#each comments as comment}
        <p>
          ID: <em>{comment.ref.id}</em>
        </p>
        <p>
          {comment.text}
          <button on:click={() => comment.ref.delete()}>Delete</button>
        </p>
        {/each}
        
        
        <button
        on:click={() => commentsRef.add({
          text: '<input type="text" class="comment">',
          createdAt: Date.now()
        })}>
            Add Comment
          </button>
          
          <span slot="loading">Loading comments...</span>
          
        </Collection>
      </Doc>
      <StorageRef {path} let:downloadURL let:ref meta let:metadata> 
  
        <img src={downloadURL} />
    
        <div slot="loading">
            Loading...
        </div>
    
        <div slot="fallback">
            Error
        </div>
    
        </StorageRef>
      </User>
    </FirebaseApp>

</main>


<!-- Styles -->
<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
  

  h1,
  em {
    color: #ff3e00;
  }

  hr {
    height: 1px;
    border: none;
    background: rgb(195, 195, 195);
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>