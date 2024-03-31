<script lang="ts" context="module">
  export type Message = {
    id: string
    username: string
    color: string
    msg: string
  }
</script>

<script lang="ts">
  import { onMount, onDestroy } from 'svelte'
  import tmi, { type Client, type ChatUserstate } from 'tmi.js'
  import Messages from './lib/Messages.svelte'
  import ColorHash from 'color-hash'

  let client: Client
  let messages: Message[] = []
  let channels: string[] = []
  const colorHash = new ColorHash()
  const addMessage = (message: Message) => {
    messages = [...messages, message].slice(-10)
  }

  const getChannels = () => {
    const channelsParam = new URLSearchParams(window.location.search).get('channels')
    return channelsParam ? channelsParam.split(',') : []
  }

  onMount(() => {
    channels = getChannels()
    client = new tmi.Client({ channels })
    client.connect()

    client.on('message', (channel: string, tags: ChatUserstate, message: string, self: boolean) => {
      addMessage({ id: tags.id!, username: tags['display-name']!, color: colorHash.hex(tags['display-name']!), msg: message })
    })
  })

  onDestroy(() => {
    if (client) client.disconnect()
  })
</script>

<main>
  {#if channels.length === 0}
    <p>Channels param not found</p>
    <p>Examples: ?channels=ibai || ?channels=ibai,pokimane</p>
  {:else}
    <div class="list">
      <Messages messages={messages}/>
    </div>
  {/if}
</main>