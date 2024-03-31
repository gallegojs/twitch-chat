<script lang="ts">
  import { scale } from 'svelte/transition'
  import { flip } from 'svelte/animate'
  import type { Message } from '../App.svelte'

  export let messages: Message[]
</script>

<ul>
  {#each messages as message, i (message.id)}
    <li animate:flip="{{ duration: 300 }}" out:scale="{{ duration: 250 }}" in:scale="{{ duration: 250 }}">
      <b style="color: {message.color}">{message.username}:</b> {message.msg}
    </li>
  {/each}
</ul>

<style>
  ul {
    text-align: left;
    list-style: none;
    overflow-wrap: break-word;
  }
  li {
    transition: opacity 0.3s ease;
  }
</style>