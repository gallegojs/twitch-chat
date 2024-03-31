<script lang="ts">
  import { scale } from 'svelte/transition'
  import { flip } from 'svelte/animate'
  import type { Message } from '../App.svelte'

  export let messages: Message[]
</script>

<ul>
  {#each messages as message, i (message.id)}
    <li animate:flip="{{ duration: 150 }}" out:scale="{{ duration: 100 }}" in:scale="{{ duration: 100 }}">
      <b style="color: {message.color}">{message.username}:</b> {message.msg}
    </li>
  {/each}
</ul>

<style>
  ul {
    text-align: left;
    list-style: none;
    overflow-wrap: break-word;
    padding: 0;
    text-shadow: 0px 0px 2px rgba(0,0,0,0.7);
  }
  li {
    transition: opacity 0.3s ease;
  }
</style>