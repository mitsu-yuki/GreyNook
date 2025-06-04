<script lang="ts">
  let identifier = '';
  let password = '';
  let text = '';
  let accessJwt = '';
  let did = '';
  let loggedIn = false;

  async function login() {
    const res = await fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ identifier, password })
    });

    if (!res.ok) {
      alert('Login failed');
      return;
    }

    const data = await res.json();
    accessJwt = data.accessJwt;
    did = data.did;
    loggedIn = true;
  }

  async function post() {
    const res = await fetch('/api/post', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ text, accessJwt, did })
    });

    if (!res.ok) {
      const err = await res.text();
      alert('Post failed: ' + err);
      return;
    }

    text = '';
    alert('Post successful');
  }
</script>

{#if !loggedIn}
  <h1>Login to BlueSky</h1>
  <input placeholder="Handle or email" bind:value={identifier} />
  <input type="password" placeholder="App password" bind:value={password} />
  <button on:click={login}>Login</button>
{:else}
  <h1>Create Post</h1>
  <textarea rows="5" cols="50" bind:value={text} placeholder="Write your post here" />
  <br />
  <button on:click={post}>Post</button>
{/if}
