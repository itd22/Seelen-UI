<script lang="ts">
  import { onMount } from 'svelte';
  import { readTextFile, BaseDirectory } from '@tauri-apps/api/fs';

  let txtContent: string = 'Loading text configuration...';
  let errorMsg: string = '';

  const CONFIG_NAME = 'right_bar_settings.txt';

  onMount(async () => {
    try {
      // Access text safely inside user application data directories via Tauri
      txtContent = await readTextFile(CONFIG_NAME, {
        dir: BaseDirectory.AppConfig
      });
    } catch (err) {
      console.error(err);
      errorMsg = 'File not found.';
      txtContent = 'Default Configuration State.\nCreate right_bar_settings.txt inside AppConfig to edit.';
    }
  });
</script>

<main class="right-bar-layout">
  <div class="inner-container">
    {#if errorMsg}
      <span class="alert">{errorMsg}</span>
    {/if}
    <article class="content-block">
      <pre>{txtContent}</pre>
    </article>
  </div>
</main>

<style>
  .right-bar-layout {
    width: 20vw;
    height: 100vh;
    position: fixed;
    top: 0;
    right: 0;
    box-sizing: border-box;
    background-color: var(--seelen-bg-primary, #151515);
    border-left: 1px solid var(--seelen-border, #2a2a2a);
    color: var(--seelen-text, #eaeaea);
    overflow-y: auto;
  }

  .inner-container {
    padding: 1.25rem;
    font-family: var(--seelen-font, system-ui, sans-serif);
  }

  .content-block pre {
    white-space: pre-wrap;
    word-break: break-all;
    margin: 0;
    font-family: inherit;
    font-size: 0.95rem;
    line-height: 1.5;
  }

  .alert {
    color: #ff5c5c;
    font-size: 0.8rem;
    display: block;
    margin-bottom: 0.5rem;
  }
</style>
