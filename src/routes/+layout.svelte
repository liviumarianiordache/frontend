<!-- src/routes/+layout.svelte -->
<script>
  // @ts-nocheck
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';

  const menuOpen = writable(false);

  onMount(() => {
    document.addEventListener('click', handleDocumentClick);

    return () => {
      document.removeEventListener('click', handleDocumentClick);
    };
  });

  function toggleMenu() {
    menuOpen.update(value => !value);
  }

  function handleDocumentClick(event) {
    closeMenuOnClickOutside(event);
  }

  function closeMenuOnClickOutside(event) {
    const menuWrapper = document.getElementById('gn-menu-wrapper');

    if (menuWrapper && !menuWrapper.contains(event.target) && $menuOpen) {
      toggleMenu();
    }
  }
</script>

<style>
  .gn-menu-main {
    position: fixed;
    width: 100%;
    top: 0;
    left: 0;
    z-index: 1000;
    height: 60px;
    background-color: #fff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }

  .gn-menu {
    cursor: pointer;
    padding: 20px;
    border: none;
    background: none;
  }

  .gn-menu-list {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;
    transform: translateY(-100%);
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    flex-direction: column;
    background-color: #fff;
    transition: transform 0.3s ease;
    visibility: hidden;
  }

  .menu-open .gn-menu-list {
    transform: translateY(0);
    visibility: visible;
  }

  .gn-menu-item {
    padding: 10px;
    text-align: center;
    border-bottom: 1px solid #ccc;
  }

  .gn-icon {
    text-decoration: none;
    color: #333;
    font-size: 18px;
  }

  .menu-open {
    background-color: #f0f0f0;
  }
</style>

<main>
  <div id="gn-menu-wrapper" class:menu-open={$menuOpen} class="gn-menu-main">
    <div class="gn-menu" on:click={toggleMenu}>
      <div class="gn-menu-list">
        <div class="gn-menu-item">
          <a href="#" class="gn-icon gn-icon-archive">Arhivă</a>
        </div>
        <div class="gn-menu-item">
          <a href="#" class="gn-icon gn-icon-download">Descărcare</a>
        </div>
      </div>
    </div>
  </div>

  <slot></slot>
</main>
